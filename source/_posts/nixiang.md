---
title: APP逆向总结
date: 2026-08-13
categories:
  - 逆向
tags:
  - 破解
---


### 苹果APP逆向
总结：如果要破解最新版的宝宝巴士IPA，不能直接从爱思助手下载正版IPA，因为FairPlay是加密的（cryptid=1），必须在已越狱或安装了巨魔的设备上安装最新版APP，然后从内存中DUMP出来未解密版的离线IPA安装包，这种丢给AI才能注入插件解锁会员。运气好的话公网上刚好有人分享了最新版APP破解版，不过一般都要掏钱，还不如官方购买SVIP。
<!--more-->

### iOS破壳软件下载

破壳是指将APP从sandbox中提取出来，FairPlay是未加密的，便于你后续逆向/VIP破解/注入操作，然后用个人ID进行签名安装即可。


| 网站 | 链接 | 推荐指数 | 备注 |
|------|------|----------|------|
| Decrypt | [https://decrypt.34306.lol/](https://decrypt.34306.lol/) | ⭐⭐⭐⭐⭐ | 免费可用，有最新版本，**强烈推荐** |
| IPA Store | [https://ipa.store/dump](https://ipa.store/dump) | ⭐⭐⭐⭐ | 免费可用，版本约滞后半年，非实时更新 |
| ArmConverter | [https://armconverter.com/store/us/](https://armconverter.com/store/us/) | ⭐⭐⭐ | 免费可用，有最新版本砸壳，缺点：排队需等待数天 |
| iPadump | [https://ipadump.com/](https://ipadump.com/) | ⭐⭐⭐ | 一分钱下载一次，性价比可，有最新版本，作为备选 |

### AI软件绕过破限
#### 1.WorkBuddy
- 使用 **Resource Hacker** 修改 EXE 的 **PE 元数据** 和 **进程名**。
- 注意：原始默认安装目录的文件夹名称中 **不能包含 `workbuddy`** 字样。

#### 2.Codex桌面版
- 修改的是 `resources` 目录里的进程名，**只改文件名即可**。
- 同时需要设置环境变量，指向改名后的可执行文件：
```bash
setx CODEX_CLI_PATH "D:\OpenAI\app\resources\coddex.exe"
```

#### 3.CodeX CLI
npm包一键全局安装，需要修改 **两处**：
**第一处：重命名可执行文件**
将以下路径中的 `codex.exe` 改为 `cx.exe`：
`C:\Users\{用户名}\AppData\Roaming\npm\node_modules\@openai\codex\node_modules\@openai\codex-win32-x64\vendor\x86_64-pc-windows-msvc\bin\codex.exe`
**第二处：修改启动脚本中的硬编码路径**
`C:\Users\{用户名}\AppData\Roaming\npm\node_modules\@openai\codex\bin\codex.js` 中写死了 `codex.exe`，将其改为 `cx.exe` 即可。最后在命令行敲codex启动。

#### 4.Claude Code
不使用npm包安装，推荐用官方的一条命令ps1脚本安装
- 与 WorkBuddy 方式相同：只改 `~\.local\bin\claude.exe` 的 **PE 元数据** 和 **进程名**（例如改为 `cc.exe`）。
- 如果仍想使用 `claude` 命令，可在同目录下创建一个同名跳转脚本 `claude.cmd`，这样敲 `claude` 时实际调用的仍是 `cc.exe`。
