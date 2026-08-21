---
title: Hexo命令
date: 2019-11-17 16:24:32
categories: 
- Hexo
tags:
- Hexo命令及安装
---

### hexo常见操作
`hexo new "postName" `#新建文章
`hexo new page "pageName" `#新建页面<!--more-->
`hexo clean` #清除部署緩存
`hexo n == hexo new` #新建文章
`hexo g == hexo generate` #生成静态页面至public目录
`hexo s == hexo server` #开启预览访问端口（默认端口4000，可在浏览器输入localhost:4000预览）
`hexo d == hexo deploy` #将.deploy目录部署到GitHub
`hexo g -d` #生成加部署
`hexo g -s `#生成加预览


### 新建文章，默认的注释内容
hexo/scaffolds/post.md

### 用户自定义样式
hexo/themes/next/source/css/_custom/custom.styl

### 文章信封区域宽度调整
source/css/_variables/custom.styl

### 完整部署流程
- [超详细Hexo+Github博客搭建小白教程](https://zhuanlan.zhihu.com/p/35668237/)

### Q&A
2022.3.15后已禁止RSA key with SHA-1，即老旧命令`ssh-keygen -t rsa -C "your@email.com"(填写github对应的邮箱)`生成的id_rsa+id_rsa.pub已失效。需用最新的命令来生成公私钥对
`ssh-keygen -t ed25519 -C "heyaff@sina.com"`
