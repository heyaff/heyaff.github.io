---
title: 电视TV软件瞎折腾
categories:
  - 电视TV
tags:
  - 电视TV
date: 2023-04-23
---

2023.03购买Sony TV后，剩下的就是无尽的折腾... ...

### 点亮杜比视界
**索尼自带播放器**
有限制，只能播放mp4格式的杜比。播放非杜比的4K视频有时候还无法播放

**当贝播放器**
不支持阿里云盘在线播放杜比，最高支持4K（会员）<!--more-->

**kodi**
推荐，但官方版的kodi无法点亮MKV等格式杜比世界，推荐kodi魔改版

Kodinerds魔改版下载地址（*不带fireTV标识*）
https://repo.kodinerds.net/index.php?action=list&scope=cat&item=Binary%20(armeabi-v7a)

- 改为中文界面
默认语言那里只有English，需要在`Add-ons/Install from repository/Look and feel/Language`中下载中文语言包就展示了
- 在线加载中文字幕
安装OpenSubtitles.org插件，设置自己的账号密码，在“kodi/设置/播放器”那里，记得改“下载字幕语言为Chinese”
- 电影/电视剧的刮削器
不翻墙的情况下，kodi自带的TV

### 无损播放阿里云盘4K视频
任意一台设备安装WebDav服务，当前我用家里闲置的Reno手机，当然也可以在电视盒子或索尼电视上安装，然后由kodi读取WebDav的云盘文件（类似共享文件夹）。
听说WebDav模式会频繁的读写、删除硬盘，损害安装的设备😳

阿里云WebDav下载 https://github.com/eritpchy/aliyundrive-webdav/releases

### TVBOX
最新版的TVBOX搜索功能有异常，故安装的20221203旧版
https://taobao.lanzouo.com/b08zl62eh
接口地址
https://dxawi.github.io/0/0.json
https://agit.ai/Yoursmile7/TVBox/raw/branch/master/XC.json

```
//老司机
https://agit.ai/ccy/v/raw/branch/master/v
https://raw.gitmirror.com/kimcrowing/IPTV/master/CCTV/zh.json
https://raw.gitmirror.com/2hacc/TVBox/main/h/h.json
https://raw.gitmirror.com/sunkangfu/666/main/a.json
https://raw.gitmirror.com/FongMi/CatVodSpider/main/json/adult.json
https://raw.gitmirror.com/gaotianliuyun/gao/master/9918.json

//综合
https://raw.gitmirror.com/liu673cn/box/main/m.json
https://kjsc0310.github.io/tvy/jk9.json
https://agit.ai/n/b/raw/branch/a/b/c.json
https://raw.gitmirror.com/sunkangfu/666/main/演唱会.json
```

### IPTV
在线看电视直播，推荐运营商的IPTV，其次是神鸟直播/Tivimate
```
### m3u直播源
https://raw.fastgit.org/imDazui/Tvlist-awesome-m3u-m3u8/master/m3u/%E7%99%BE%E8%A7%86TV.m3u
### IPTV节目预告epg接口
https://epg.112114.xyz/pp.xml
待定http://diyp.112114.xyz/pp.xml
http://epg.51zmt.top:8000/e.xml
### 老司机
https://raw.fastgit.org/YanG-1989/m3u/main/Adult.m3u
```


