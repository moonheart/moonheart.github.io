---
title: "MIUI 系统修复音频延迟"
date: 2019-01-25T23:19:23+08:00
draft: false
---

在MIUI系统中，在玩某些音游（Arcaea）的时候，需要将延迟调的很高（200\~300ms），才能使音画同步，而在原生系统中延迟却非常低（10\~30ms）。

这一现象在我的小米6和小米平板4 plus中都遇到了。

之后我在Android官方文档中看到关于[音频延迟时间](https://developer.android.com/ndk/guides/audio/audio-latency?hl=zh-cn)的说明，其中提到：

> 当前没有 API 可以在运行时确定 Android 设备上通过任何路径的音频延迟时间。 不过，您可以使用下列硬件功能标记了解设备是否能为延迟时间提供任何保证：<br/>
android.hardware.audio.low_latency 指示 45 毫秒或更短的持续输出延迟时间。
> android.hardware.audio.pro 指示 20 毫秒或更短的持续往返延迟时间。

随后通过搜索，在MIUI论坛中发现了这篇帖子【[利用android新特性 减低声音延迟](http://www.miui.com/thread-9688302-1-1.html)】。

通过对比发现，原生系统中的`/vendor/etc/permissions/`下有`android.hardware.audio.low_latency.xml`、`android.hardware.audio.pro.xml`这两个文件，猜测是这两个文件标记了设备是否支持这两个特性，而这也与上文提到的文档中的描述一致。

手动将这两个文件复制到MIUI系统的`/system/vendor/etc/permissions/`中后，音频延迟问题解决了。

猜测：一些应用为了降低音频延迟，会检查设备是否支持高性能音频，如果不支持的话会采用传统的延迟更大的方式进行音频输出。

我制作了一个[Magisk模块](https://github.com/moonheart/moonheart.github.io/raw/master/files/magisk_fix_audio_latency.zip)可以按照文中提供的方式对MIUI的音频延迟进行修复。

副作用：如果设备运行内存不足，可能会导致这些支持高性能音频的应用的声音卡顿爆音，这也是我在使用原生系统的时候经常遇到的，清理一下后台程序并重启应用就能解决。