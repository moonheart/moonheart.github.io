---
title: "切换到 Debian sid 使用一个月的体验"
date: 2022-05-17T22:10:52+08:00
draft: false
---

一个月前，由于 docker 使用体验不佳，加上当时正好遇到一个项目需要使用 Linux 环境编译，但是 WSL/WSL2 有一些小问题一直没解决，就顺势从 Windows 11 切换到了 Linux。

## 选择哪个发行版？

最开始我是准备用 ArchLinux 的，因为她的滚动更新策略我比较喜欢，以前使用过一个星期，由于一些电源管理的问题，本人太菜一直没修好，于是放弃了；公司服务器主要使用的是 Debian 9/10, 考虑到贴和工作环境，于是我选择了最新 的Debian 11。

## 遇到的问题

### 网卡不工作

安装 Debian 11 的时候，遇到一个哭笑不得的问题：Debian 11 的 ISO 自带的内核是 5.10, 当时安装的时候就发现不对劲，Wi-Fi 不工作；一番搜索之后才发现我的笔记本网卡是 MT7921，要从 5.15 内核才支持。又是搜索了一番，发现原来 Debian 也有滚动更新的版本，于是下载了 Debian sid 的镜像进行安装。

### S3 睡眠失败

安装好用了一两天之后发现：睡眠了之后无法唤醒？点击睡眠之后无论是键盘鼠标还是电源键都无法唤醒，只能长按电源键断电。最开始以为是驱动问题，但是尝试各种方式无果，最后经过 TG 群的一位群友提醒，发现是因为设备太新，不支持 S3 睡眠，取而代之的是 Windows 的 新式待机：[https://docs.microsoft.com/zh-cn/windows-hardware/design/device-experiences/modern-standby-vs-s3](https://docs.microsoft.com/zh-cn/windows-hardware/design/device-experiences/modern-standby-vs-s3)，经过一番搜索，找到了可用的方案：[https://dev.to/epassaro/fix-suspend-issues-on-dell-7405-2-in-1-3l1b](https://dev.to/epassaro/fix-suspend-issues-on-dell-7405-2-in-1-3l1b)，通过修补 DSDT 来修复 S3 睡眠。

### Windows 软件问题

在国内无法避免的要使用一些通讯软件，如 微信，钉钉，企业微信等等，根据我的经验，Wine 下运行的软件或多或少都会有些小问题，最后决定采用 VM 方案，在 VirtualBox 中安装了一个精简版的 Windows 7 来运行这些软件，分配了 2 GB 内存，平时使用基本够用。

其他都是一些小问题，网上解决方案很多，这里就不再赘述了。

## 好的体验

首先我要由衷的感叹：**Linux 下的 docker 真是太好用了！**超级顺滑，再也不用那个超级重的 Docker Desktop For Windows 了；而且挂载目录也不用担心 IO 性能问题了；

另外就是开发的体验变好了：可以很方便的使用一些之前在 Windows 下用起来很麻烦的命令，比如 make, gcc, 之前只能在 WSL 中使用；

并且 Linux 下有统一的 shell 环境，不像 Windows 有 CMD/Powershell/WSL 还有为了使用一些 Linux 工具安装的 MSYS2/Cygwin 等等；

另外 Linux 下有统一的包管理器，Windows 下我需要 Chocolatey/scoop/winget 换着用。

常用的工具在 Linux 下都有：Jetbrains 全家桶，Lens，vscode，dbeaver，telegram，utools，Edge 等等，刚需的 Onedrive 有 onedriver 代替，clash for windows 换成了 systemd 管理的 clash。

## 不好的体验

一些软件没有 Linux 版本，只能在 VM 中使用；

偶尔遇到一些奇怪的问题，比如开机后 USB 键鼠失效，只能关机后开机解决；

KDE 在拔出外接显示器再重新连接后，窗口全部挤在笔记本屏幕上；

更新内核后 VMWare 的内核模块需要重新手动编译，我嫌麻烦就换 VirtualBox 了；

以上都是我在一个月左右的体验，其中可能会有一些看起来很初级的问题，望各位体谅；最后附上系统信息：

```
       _,met$$$$$gg.          moon@tb14p-debian 
    ,g$$$$$$$$$$$$$$$P.       ----------------- 
  ,g$$P"     """Y$$.".        OS: Debian GNU/Linux bookworm/sid x86_64 
 ,$$P'              `$$$.     Host: 20YN Lenovo ThinkBook 14p Gen 2 
',$$P       ,ggs.     `$$b:   Kernel: 5.17.0-2-amd64 
`d$$'     ,$P"'   .    $$$    Uptime: 10 hours, 48 mins 
 $$P      d$'     ,    $$P    Packages: 2957 (dpkg) 
 $$:      $$.   -    ,d$$'    Shell: zsh 5.8.1 
 $$;      Y$b._   _,d$P'      Resolution: 2240x1400 
 Y$$.    `.`"Y$$$$P"'         DE: Plasma 5.24.5 
 `$$b      "-.__              WM: KWin 
  `Y$$                        Theme: [Plasma], Breeze [GTK2/3] 
   `Y$$.                      Icons: [Plasma], breeze [GTK2/3] 
     `$$b.                    Terminal: konsole 
       `Y$$b.                 Terminal Font: FiraCode Nerd Font Mono 10 
          `"Y$b._             CPU: AMD Ryzen 7 5800H with Radeon Graphics (16) @ 3.200GHz 
              `"""            GPU: AMD ATI 04:00.0 Cezanne 
                              Memory: 14509MiB / 28002MiB
```