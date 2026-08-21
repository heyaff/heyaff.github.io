---
title: Nginx
categories:
  - Nginx
date: 2020-03-04 19:01:48
tags:
  - nginx
---

### 常用命令
__启动nginx__：`nginx`
__关闭nginx__：`nginx -s stop`
__检测配置格式__：`nginx -t`
__重启nginx__：`nginx -s reload`

![Nginx转发](/images/nginx转发.jpg)
<!--more-->
### upstream
后端的应用IP
对用户来说是透明的，没有暴漏后端请求

### server
一个站点/域名，nginx可配置十多个域名，见上图

### location

`~`开头   表示区分大小写的正则匹配
```
location ^~ /images/ {
# 匹配任何已/images/开头的任何查询并且停止搜索，后面的正则不会再匹配
}
```
`~*`开头   表示不区分大小写的正则匹配
```
location ~* .(gif|jpg|jpeg)$ {
# 匹配任何已.gif、.jpg 或 .jpeg 结尾的请求
}
```
`!~`   区分大小写不匹配的正则
`!~*`  不区分大小写不匹配的正则

```bash
localtion / {
    # 所有请求都匹配以下规则
    # 因为所有的地址都以 / 开头，所以这条规则将匹配到所有请求
    # xxx 你的配置写在这里
}

location = / {
    # 精确匹配 / ，后面带任何字符串的地址都不匹配这条
}

localtion /api {
    # 匹配任何 /api 开头的URL，包括 /api 后面任意的, 比如 /api/getList
    # 匹配符合以后，还要继续往下搜索
    # 只有后面的正则表达式没有匹配到时，这一条才会采用这一条
}

localtion ~ /api/abc {
    # 匹配任何 /api/abc 开头的URL，包括 /api/abc 后面任意的, 比如 /api/abc/getList
    # 匹配符合以后，还要继续往下搜索
    # 只有后面的正则表达式没有匹配到时，这一条才会采用这一条
}


location /cn {
    #proxy_pass跳转，浏览器上URL不会跳转，用户感知不到
    #proxy_pass最后有斜杠，最终跳转http://heyaff.github.io/index.html
    #proxy_pass最后无斜杠，最终跳转http://heyaff.github.io/cn/index.html
    proxy_pass http://heyaff.github.io;
}
```


### Nginx生产环境场景
1、静态文件（html/css/js/图片/字体等）直接在Nginx对应目录下访问
2、其它（api访问）代理到对应api服务的端口
