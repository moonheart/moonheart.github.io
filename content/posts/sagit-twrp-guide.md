---
title: "小米 6 刷 twrp 教程"
date: 2017-10-06T22:14:23+08:00
draft: false
---

准备：电脑，小米6一台，数据线

1. 下载线刷工具

进入 http://www.miui.com/shuaji-393.html ，选择通用刷机工具点击下载, 然后安装。

2. 下载recovery

进入 https://dl.twrp.me/sagit/ 选择最新的下载

3. 放好recovery文件

进入刷机工具安装目录， 默认 C:\XiaoMi\XiaoMiFlash\, 然后进入 Source\ThirdParty\Google\Android, 把刚刚下载的twrp-3.1.1-1-sagit.img放到这个目录。asad

4. 新建刷recovery脚本

新建文本文件，修改名字为flashrecovery.bat，内容如下

fastboot flash recovery twrp-3.1.1-1-sagit.img
fastboot boot twrp-3.1.1-1-sagit.img

5. 重启到fastboot

长按音量增 + 电源键 ，等待 手机重启进入fastboot，然后将手机链接到电脑。

6. 双击flash_recovery.bat文件，这时手机会自动重启进入recovery。