---
title: nmap及linux命令
date: 2020-12-29
categories:
  - nmap
  - curl
  - awk
tags:
  - 端口扫描
  - curl
---


### PN No ping扫描
如果远程主机有防火墙IDS和IPS系统，你可以使用-PN命令来确保不ping远程主机，因为有时候防火墙会禁用ping
禁ping，TCP端口扫描命令：
`nmap -sT -Pn 目标IP`


### CURL命令<!--more-->
- 添加请求头
```
curl -X GET "https://xxx.com" \
     -H "Cookie: xxx" \
     -H "Referer: https://xxx.com:1111/xxx"
```
- POST添加请求body -d
`curl https://xxx.com -X POST -d 'id=1&user=hehe'`


### Linux下awk命令
用来截取文本中的片段，类似excel的分列


```python
#使用多个分隔符.先使用空格分割，然后对分割结果再使用","分割，输出第1 2 5列
awk -F '[ ,]'  '{print $1,$2,$5}'   log.txt

#使用","分列后，统计第7 9列出现的次数，输出最多的TOP 10
cat log.txt | awk -F ',' '{print $7,$9}' | sort | uniq -c | sort -k1nr | head -10
```





