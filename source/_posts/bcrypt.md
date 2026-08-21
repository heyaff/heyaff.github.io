---
title: bcrypt最佳实践
date: 2021-03-02
categories:
  - bcrypt
tags:
  - bcrypt
---


[bcrypt在线生成工具](https://bcrypthashgenerator.tool-kit.dev/)


### bcrypt组成
![Bcrpt密文](/images/bcrypt.png)
+ `2a`:算法标识符
+ `10`:轮数，默认是10，即2^10=1024次迭代<!--more-->
+ `YET3WUXkOBCwKxOzLha2MO`:原始salt值是`heyaff@123456789`16字节128bits，你看到的YET开头是经OpenBSD-Base64编码后22字符，故**盐值可逆**
+ `VaGgw3duNuPMQqx1LUV4CoXxTHUJihO`：哈希后的密文是23字节184bit，经OpenBSD-Base64编码后，你看到的是31字符

### bcrypt特征
默认情况下，salt随机生成，故同一个密码，bcrypt每次生成的hash都不一样
- 慢散列hash算法，单向不可逆
- 轮数固定，bcrypt时间也是固定的，默认是10，只需300~500ms
- 轮数和salt都固定，同一个密码多次bcrypt的结果不会变
- 轮数越大，时间指数级增加
- bcrypt比MD5慢的太多，有效防止彩虹表攻击

### 后端执行bcrypt的实践方案
![Bcrypt用在后端](/images/bcrypt_3.png)
i. 前端js对口令执行MD5后，再用后端公钥进行加密
ii. 后端用私钥解密，得到口令的MD5值，执行bcrypt≈SHA256加盐散列，保存到数据库中

小技巧：
公钥加密前，必须对口令执行哈希散列，要不然后端内存中会有明文口令

### 前端执行bcrypt的最佳实践

不推荐后端服务器执行bcrypt，影响服务器性能和并发，故推荐前端慢速加盐散列，百毫秒级的延时，对用户的影响几乎忽略不计，来对抗暴力破解和撞库
![Bcrypt最佳实践](/images/bcrypt1.png)
i. 前端js对用户的口令执行bcrypt，盐值固定为'用户名'+'口令'+'固定字符串'拼接而成，伪代码如下
```
salt = '$2a$10$' + String(cryptojs.SHA256('Heyaff' + username + password)).substring(0,22);
```
ii. 后端接收到bcrypt密文，执行SHA256加盐散列，后端盐值是在用户注册时随机生成的，并写入数据库，修改口令时会重新生成新的盐值
iii. 后端校验时，接收到前端的bcrypt密文，读取后端salt，执行SHA256(bcrypt,salt)与数据库比对。

小技巧：
1、固定盐值，使用户的bcrypt每次都固定，同时也保证了不同用户不同盐值
2、前端盐值是可逆的，不能把口令明文写入salt，所以执行了SHA256散列并截取固定长度
3、传给后端的bcrypt既然是固定不变的，它也相当于“替代口令”，按传统意义的加盐散列存储即可
4、校验时，后端没有执行bcrypt算法

### Q&A
Q: SHA256代表多少字节，多少位(bit)？
A: SHA256是256bit长的哈希值，32个字节。你常看到的是十六进制字符串来展示的，是64个十六进制字符串，即两个Hex十六进制字符=一个字节。
![SHA长度](/images/SHA256.png)

