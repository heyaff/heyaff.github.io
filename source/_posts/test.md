---
title: 渗透测试技巧
date: 2021-02-19
categories:
  - 渗透测试
tags:
  - 渗透工具
---


### 目录扫描爆破
Dirsearch
`python3 dirsearch.py -u https://xxx -x 403,404,401`
可选：
`-f -e php,html,jspx`，知道网站的应用架构后，限定扫描某后缀
`--remove-extensions`，去掉字典中所有的后缀，探测子目录

https://github.com/maurosoria/dirsearch
思路：爆破出一个目录后，继续对二级/三级目录进行爆破<!--more-->

### SQLMAP注入
注入点位置，加*丢给SQLMAP跑<font color='Blue'>（如果是BP手工测试，注入点位置是加'，看有无SQL报错信息）</font>

```
--batch 不会询问你输入 全部默认确定
-v	sql详细输出级别，等级3会显示payload

python sqlmap.py -r demo.txt --dbs --batch -v 3

python sqlmap.py -r demo.txt --tables -D 库名

python sqlmap.py -r demo.txt --columns -T 表名 -D 库名

python sqlmap.py -r demo.txt --dump -C 列名1,列名2 -T 表名 -D 库名

python sqlmap.py -r demo.txt --dump-all -T 表名 -D 库名
```

### 前端VUE项目泄露未授权接口
Webpack所生成的js有他的命名规律，一般都为`xxx.随机字母数字.js`的两段式，且每一个JS文件具体内容均以`webpackJsonp`开始。

--------
**目标1：获取一个站点下所有的js文件**
工具①：burp
在Target标签下，选择站点，点进去右键`Copy selected URLs`，复制js script的全部URL
工具②：F12 控制台
--------
**目标2：提取js中隐藏的接口**
工具③：LinkFinder
`python3 linkfinder.py -i https://xxx.com/js/xxx.min.js -o cli`，目前不支持批量查询，一条条查吧
工具④：JSFinder
`python3 JSFinder.py -u https://xxx.com/js/xxx.min.js`
--------

如果URL中有<font color=Blue>/#/</font>，基本是Webpack打包的，若出现SpringBoot的**Whitelabel Error Page**报错，尝试URL最后加个/

### 敏感信息泄露
前端JS（如app.1.xxxxxx.js）中，不止隐藏的接口，可能还有OSS的密钥，AES的密钥（搜索encrypt）

### sourcemap文件泄露，还原源代码
查看前端js文件的末尾，可能会有注释的.js.map文件，下载后，用reverse-sourcemap工具还原出项目代码，从而获取敏感接口等数据。如不想下载可直接F12查看webpack://目录
```
reverse-sourcemap -v app.77f99089.js.map -o source
```

### 泄露源代码
在黑盒登录界面，找不到突破口的话，可以尝试在Github上搜索源代码，<font color=Blue>不要搜索中文注释或HTML中的代码</font>，尽量去看/scripts/xxx.js中源码，如某函数中“/api/server/getkey_pc”，在GitHub上搜`getkey_pc`关键词，找出类似源代码，找出供应商etc.

### 上传Webshell
比如将php文件的`Content-Type:application/octet-stream`改为
```
Content-Type: text/html
Content-Type: image/png
Content-Type: text/xml
```
来绕过服务器白名单限制，实现上传webshell
