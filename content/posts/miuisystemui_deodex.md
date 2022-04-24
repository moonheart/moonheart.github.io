---
title: "MIUI 官方系统 deodex MIUI SystemUI"
date: 2018-09-24T23:18:23+08:00
draft: false
---

之前发的修改国际版通知的教程中涉及到了修改MIUISystemUI.apk，好多人说找不到classes.dex文件，因为用的是官方系统。但是当时我用的是官改系统，整个系统都是deodex的，所以可以直接修改。

接下来我会讲解如何官方系统修改MIUISystemUI，理论上适用于其他系统apk（。

## 准备工作:
1. Windows PC
2. baksmali-xxx.jar, smali-xxx.jar，到这里下载最新的 https://bitbucket.org/JesusFreke/smali/downloads/
3. adb的基本操作（推荐将adb和fastboot加入环境变量）
4. Java环境（百度去）
## 操作步骤：
1. 手机开启adb调试，连接到电脑，确保驱动正确安装。
2. 随便找一个地方建立文件夹，比如【miui】，将下载的baksmali-xxx.jar, smali-xxx.jar复制到这里。
3. 启动命令行，进入【miui】文件夹，把需要的文件拉取到电脑上。执行命令：
4. 反编译MiuiSystemUI.odex,会生成一个out目录。执行命令：
5. 按需修改out目录中的smali文件。如修改国际版通知等等。
6. 回编译smali文件成classes.dex, 执行命令：
7. 用解压缩软件打开MiuiSystemUI.apk,将生成的classes.dex文件拖入其中，你就有一个deodex的MiuiSystemUI.apk了
8. 直接替换文件到system或者使用magisk进行替换。