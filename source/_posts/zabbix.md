---
title: zabbix漏洞
date: 2020-03-31
categories:
  - 应急响应
tags:
  - zabbix
---


### Zabbix（扎比克斯）
用来监控IDC服务器运行状态，告警
zabbix agent部署在所有服务器上，采集数据上报给master。zabbix的web界面是php，默认账号密码admin/zabbix
未授权访问漏洞：
http://xxx/zabbix/zabbix.php?action=dashboard.view&ddreset=1
攻击者可绕过登陆页面直接访问仪表板页面，即匿名访问系统/网络环境数据<!--more-->

### 安全建议
1、禁止Zabbix暴露在公网
2、zabbix的登录口令一定要复杂，立即修改默认口令或弱口令
3、zabbix的server和agent都不要以root启动，不要设置AllowRoot=1
4、禁止agent执行system.run，不要设置EnableRemoteCommands=1
5、经常打安全补丁，保持系统内核版本，Zabbix版本为最新

### 攻击事件回顾
黑客通过测试环境一个后台登陆页面，通过修改XFF，绕过爆破限制得到管理员账号密码（test1/a123456）。然后在页面上传webshell探测内网，并利用默认口令（Admin/zabbix）成功登录测试环境Zabbix管理界面，该集群下约625个节点服务器。

安全应急
1、测试环境开放公网，要经过安全测试
2、管理后台禁止开放公网，加强账号强度
3、测试/开发环境未部署HIDS，入侵检测未告警
4、中间件弱口令、空口令、默认口令，引入安全鉴权机制
