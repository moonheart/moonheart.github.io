---
title: 在 linux 系统通过修改 DSDT 修复 S3 睡眠
date: 2022-04-24T01:10:23+08:00
draft: false
---

> 解决方案参考 https://dev.to/epassaro/fix-suspend-issues-on-dell-7405-2-in-1-3l1b

与原文不同的是，他的设备是 Dell 7405 2-in-1 而我的是 Thinkbook 14P, 所以 DSDT 修改略有不同。

反编译出 dsl 之后，找到如下位置：
``` dsl
    Name (NOS3, Package (0x04)
    {
        0x03, 
        0x03, 
        0x00, 
        0x00
    })
```
替换为
``` dsl
    Name (_S3, Package (0x04)
    {
        0x03, 
        0x03, 
        0x00, 
        0x00
    })
```
同时将 `DefinitionBlock ("", "DSDT", 1, "LENOVO", "AMD", 0x00001000)` 替换为 `DefinitionBlock ("", "DSDT", 1, "LENOVO", "AMD", 0x00001001)`，这里是加了个版本号，让内核可以识别到。