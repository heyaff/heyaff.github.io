---
title: 应急响应
date: 2023-08-25
categories:
  - SRC
tags:
  - 安全应急
---


### 一、排查原因
#### 1、查看ssh爆破成功记录

`tail /var/log/secure`

`grep "Accepted " /var/log/secure | awk '{print $1,$2,$3,$9,$11,$13}'`
查看登录成功的用户、登录方式及来源IP
<!--more-->
`cat /var/log/secure | awk '/Failed/{print $(NF-3)}' | sort | uniq -c | awk '{print $2" = "$1;}'`
查看登录失败的来源IP及次数


#### 2、登录用户相关，排查登录/注销、来源IP、认证方式等
`cat /var/log/secure | grep "Nov 6 22:"`
`lastlog`


#### 3、查看端口网络连接信息
`netstat -atp| grep 3000`

#### 4、查看可疑进程
1、`ps -ef`，查看进程ID，查看有哪些可疑的进程，可以继续追踪其父进程ID
2、`ps -ef |grep 父进程ID`，定位到可疑进程或者父进程后，继续查看进程是在哪个目录，哪个文件启动的
3、`ls -alt /proc/(父)进程`，输出的结果中 **cwd** 表示进程的当前工作目录，**exe** 指向进程执行的具体文件，**cmdline** 包含了启动该进程时使用的命令行参数等
4、`netstat -antp | grep 进程ID或者端口`，查看网络连接信息，是否外连。
5、`netstat -antp | grep ESTABLISHED`，如果进程ID没有外连，可以看看目前已经建立外连的其他进程
6、`ss -antp |grep 进程ID`， netstat会逐步被ss替代
7、`docker ps`，列举容器进程列表
8、`docker exec 容器ID /bin/bash`，启动交互式shell会话进入容器内部来执行命令

#### 5、查看某目录下在过去3天被修改过的文件
`find /etc/cron.d -type f -mtime -3`
`find /root/.ssh/ -type f -mtime -3`



### 二、处置动作
#### 删除定时任务
输入`crontab -e`打开crontab文件，直接删除相关行，保存退出

#### 删除authorized_keys中相关的公钥
