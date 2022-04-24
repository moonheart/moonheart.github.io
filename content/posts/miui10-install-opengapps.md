---
title: "MIUI10 刷入 opengapps"
date: 2019-01-28T17:20:23+08:00
draft: false
---

MIUI系统中没有带谷歌服务，而手动安装的非系统应用会有一些权限问题，导致奇怪的Bug，于是我考虑刷入opengapps。

这里我选择的是micro版本。

## 注意

**刷入opengapps后已经修改了system分区，这时候不能直接开机。因为内核有验证system分区完整性，直接开机会卡米。这时需要再刷入Magisk或者SuperSU对内核进行patch，才能正常开机。**

## 开机向导FC

开机后，此时开机向导已经被替换成了Google了的，这时遇到的第一个问题是开机向导过不去，因为MIUI系统对Android做了一些修改，导致Google的开机向导在在调用一些方法的时候FC。

需要禁用开机向导才能继续。

重启进入Recovery，在`/system/build.prop` 中加入如下代码后重启继续。

```
ro.setupwizard.mode=DISABLED
```

或者使用 [这个Zip包](https://github.com/moonheart/moonheart.github.io/raw/master/files/Disable_SetupWizard.zip) 刷入禁用开机向导。

## Webview丢失

开机后，很多应用FC，查看日志后发现没有检测到Webview，在开发者选项中查看Webview实现，发现是空的，怎么回事呢？因为opengapps替换了MIUI系统自带的WebviewGoogle，导致系统检测不到Webview。

这时打开opengapps安装包中的 `gapps-remove.txt` 文件，找到 `/system/app/WebViewGoogle` 这一行，把它删掉或者在前面加一个#注释掉这一行就行了。

## App被覆盖

opengapps会移除一些系统应用，即使它不是stock版本的。这时我们需要使用[配置文件](https://github.com/opengapps/opengapps/wiki/Advanced-Features-and-Options)。在配置文件中加入不想被覆盖的应用就好了。

比如日历被移除了，因为micro版本安装了Google日历，需要在配置文件中加入 `CalendarGoogle` 来防止MIUI日历被移除。

## 权限问题

如果遇到权限问题，并且无法在系统中给予权限的话，可以参考[这篇官方Wiki](https://github.com/opengapps/opengapps/wiki/Notes-for-Android-6.0#workaround-for-advanced-users)进行设置。