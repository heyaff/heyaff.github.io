---
title: HTTP脚本踩坑记录
date: 2024-06-06
categories:
  - python
tags:
  - python
  - 脚本
---

## 踩坑实战
### 多个HTTP请求包有关联，不是单独的请求
1、独立的请求是requests.post()，
2、多个请求比如一个GET一个POST这种，前后有关联的话需要用session
```
session = requests.session()
response = session.get()
```
<!--more-->

### 抓包后，Repeater重放请求，发现和原始的结果不一样，应该是这个包有刷新机制或限制，即单独POST这个包是不行的，必须重新走下完整的流程

比如POST的body如下
`username=80264631&password=xxx&lt=LT-8653057-B3MIe2zZ7U5y9&execution=e2s1&_eventId=submit&sliderWidth=0&sliderToken=0&deviceId=`
具体的参数值是从哪来的，一开始我一直在F12控制台搜索，以为是js里面的值，因为当时在找pwd的加密密钥，结果没找到，就忘了看HTML的表单数据，其实HTML中也有加密密钥 >_<

```
POST的表单
    # <form id="authenticateByTT" action="/siam/login?">
    #     <input type="hidden" name="authType" value="ENC(eoxdJKT8CIi+aJV5IrZ87Q==)"/>
    #     <input type="hidden" name="lt" value="LT-8653057-B3MIe2zZ7U5y9"/>
    #     <input type="hidden" name="execution" value="e1s1"/>
    #     <input type="hidden" name="_eventId" value="toTTAuth"/>
    #     <input type="hidden" name="mfaType" value="TT"/>
    # </form>
```


### 如果用了session请求，得到的返回包还是和真实浏览器有区别，应该是第一个GET请求包问题
比如我GET mo.adc.com，然后跳转到sso.adc.com，最后POST提交账号密码。
如果第一个GET用sso.adc.com可能有点问题，可以追溯到mo.adc.com，然后session.get(url, headers=headers,allow_redirects=True)，开启自动跳转到sso。这样完整的模拟浏览器抓包，结果是最准的。

### 重定向的坑，默认是开启重定向的，直到返回非301/302状态码。
请求顺序：POST-->302-->302-->200
坑点一：一开始我想一路重定向，默认设置，结果最终返回403，POST-->302-->403就结束了，因为302跳转到了另外的域名，即跨域请求，去掉了header中`Origin`字段，一路绿灯直达200。（去掉整个header也中^_^）
坑点二：但我想获取302中的Set-Cookie值，只能单独处理重定向，要不然返回200不是我想要的。注意，Header中要去掉Host字段，要不然跳转到其他域名会报错。
这里也有个坑点，复用之前的header字段，结果响应超时，粘贴到burp中也无法复现，但依然怀疑header的问题，去掉header后，问题才解决。
原来是`Content-Length`惹的祸，怪不得响应超时，burp中会自动帮你修改这个值
【**总结**】自定义的Header头中，尽量去掉`Origin、Content-Length`

### UA字段的坑
我直接fork别人的代码，POST返回结果和抓包不一样，原来是UA字段中`Chrome/93.0.4577.82`，改了版本号就可以了，所以要注意刷新UA字段。



## py技巧
### 列表
```
原始的response返回包
{
    "code": 200,
    "message": "OK",
    "msg": "OK",
    "data": {
        "prePageSize": 200,
        "totalNum": 3,
        "system_ops": [
            {
                "nickname": "谢子寒",
                "username": "80369018"
            },
            {
                "nickname": "李起超",
                "username": "80372465"
            }
        ],
        "lastId": "657b0723db7ed",
        "prePageNumber": 1
    },
    "logId": "8ab501f9294c",
    "status": "0000"
}
```
上面是返回包中的原始结果，直接打印system_ops不适合统计数据，优化后的代码片段：
```python
data = res.json()['data']
list=[]
for ops_user in data['system_ops']:
    userid=ops_user["username"]
    username=ops_user['nickname']
    list.append(f"{username}({userid})")
# 使用 join 将列表连接成一个字符串，每个元素用逗号分隔
out = ','.join(list)
```
输出结果
```
打印list：
['谢子寒(80369018)', '李起超(80372465)']
打印out：
谢子寒(80369018),李起超(80372465)
```
【**总结**】如果要将data中的数组元素全部打印并拼接出来，在for循环中，用列表list.append()来拼接

### 获取返回包的字段元素
上面的response，直接用`response['data']`取值可能会报错，因为有时返回包没有data字段，运行报错`KeyError`，用`response.get("data","") `更安全
注意！！！不要以为是GET请求response，别搞错了！！！

### 自定义函数的输入输出
初级的做法是传入一个IP，返回查询后的结果，如负责人、机房等value字段
推荐做法，传入传出，都可以是字典，即键值对。你可以定义out字典
out = {"type":"ip","env":"","location":"","net_region":"","system_name":""}
比如上面的response返回包，将数据赋值给out，最后自定义函数return out
**【总结】** 自定义函数可返回键值对，而不是只返回value值。

### logger函数
```
from loguru import logger
```
使用logger.info比print记录日志更加专业，生产环境适用。

