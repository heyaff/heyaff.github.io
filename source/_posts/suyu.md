---
title: 术语
date: 2021-03-17
categories:
  - 名词解释
tags:
  - 专业术语
---

### Maven
![](/images/maven.jpg "后端开发IDE阶段才会用到maven")
Maven主要做了两件事：
统一开发规范与工具
统一管理jar/aar/so包<!--more-->

Maven是一个JAVA项目管理工具，直接配置pom文件，Maven会自动帮你下载和管理jar包。
Maven项目有一个中央仓库，存了好多好多jar包，每个jar包不同的版本都存了。你想用什么jar包，只需要在Maven的配置文件中（pom.xml）配置一下，写好用什么jar包，什么版本即可，Maven会自动帮你下载jar包；你依赖的jar包如果还依赖其他jar包的话，它也可以帮你下载下来。

maven项目的目录结构遵守以下规范

| 路径 | 说明 |
| :----: | :----: |
| src/main/java | 存放项目的.java文件，开发源代码 | 
| src/main/resources | 存放项目配置文件，如spring, hibernate配置文件 | 
| src/main/webapp | 存放web项目资源文件（web项目才有） |
| pom.xml | maven项目核心配置文件 |

就是如果是一个maven项目那么它的根目录下必定存在src文件夹和pom.xml。


### Maven和Jenkins关系
Maven偏项目开发管理，主要是解决项目依懒问题；
Jenkins偏运维管理，让运维从繁琐的发布工作中解脱出来。

项目的自动化编译、打包、部署：
码农提交了代码之后（svn/git），Jenkins会自动的把代码拿下来，编译好，编译之前可能要跑测试（跑测试、编译、打包是在Maven中做的），测试通过之后项目直接打包，然后部署到服务器上。从上面看出来，很多事情并不是Jenkins单独搞，也是有集成Maven、ant、gradle的


### Node.js
前端项目，在服务器上解析运行js文件

`src`文件夹，存放前端工程师的源代码，如app.vue等源码，`dist`文件夹，存放编译好的前端文件

npm install xxx包，从仓库下载的依赖资源包都放在了node_modules目录下面

npm包是前端js资源仓库包，例xxx.min.js，xxx(hash).js

**webpack编译打包项目**，运行`npm run build`后，生成了一个dist文件夹，放在Nginx/tomcat服务器指定路径下

<center>
<div style="display:inline-block;" title="sdfasdfasdf">{% img /images/npm_dist.png 180%}前端IDE开发工具视图</div>
<div style="display:inline-block;margin-left:10px;">{% img /images/npm_dist1.png 450%}Nodejs服务器上视图</div>
</center>

### ansible
1、基于ssh运行
2、无需客户端

`Ansible` 拿来做服务器部署再合适不过。只需要**准备好一份主机清单和要执行的命令步骤**，就可以实现**对一个清单的全部主机远程 批量执行命令**。

推荐免密公钥SSH登录远程主机清单

### CI/CD流程
CI`持续集成`：源代码--->编译后的包，即BUILD。最终产物推送到镜像仓库
CD`持续部署`：包---->生产环境上线

***Dockerfile*** 用于构建一个镜像的文件（基于标准镜像改ba改ba）


1.编写代码
2.自测+测试用例
3.编写Dockerfile
4.构建Docker镜像
5.推送Docker镜像到仓库
6.编写k8s YAML 文件
7.更新 YAML 文件中的Docker镜像TAG
8.利用kubectl工具部署应用

-[中小团队基于Docker的devops实践](https://blog.ops-coffee.cn/s/gatfwneto_agsjhzdv5yzq "运维咖啡吧")


### 包部署和镜像部署
包部署
1. 在初次部署时会生成一台胖容器，里面的系统由云平台分配。
2. 如果系统中缺乏某些软件或运行环境需要自行上去安装，比如nginx
3. 每次部署变更只是不断的把软件包丢到机器里的部署目录，运行数据和自行安装的软件不会消失
4. 扩容后又得重新安装需要的软件和环境	

镜像部署
1. 可以自行制作基础镜像，把项目运行时需要的软件和环境都打到基础镜像里
2. 在构建时把软件包打到自行制作的基础镜像中，生成最终的最终镜像
3. 每次部署变更都会根据最终镜像重新生成一台机器，但ip可以保持不变
4. 每次部署变更得到的都是一个新的运行环境（纯净的），运行时的数据需要使用其他的方式来收集来保证重启不会丢失
5. 由于环境和软件包全都在镜像里，所以非常方便扩容

