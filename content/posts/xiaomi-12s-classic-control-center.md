---
title: "Xiaomi 12s Ultra 恢复经典控制中心"
date: 2022-12-24T22:00:00+08:00
draft: false
---

小米 12s 系列强制使用了新版控制中心。如果想要使用经典控制中心，可以按照如下方法修改。

用 MT 管理器找到 /system/product/overlay/DevicesOverlay.apk 并复制到内部存储如 Documents/DevicesOverlay.apk，通过 Arsc 编辑器打开 resources.arsc 文件。找到 com.miui.systemui.devices.overlay/integer/integer/force_use_control_panel，将值改为 0。

保存 apk。如果是 MIUI 14/Android 13，需要给 apk 签名才会被系统识别，使用 MT 管理器自带的签名工具即可。

最后通过 magisk 模块覆盖到系统。

下载地址：https://github.com/moonheart/NoControlPanel
