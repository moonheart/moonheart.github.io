---
title: "缓解 MIUI 杀后台"
date: 2023-01-06T22:00:00+08:00
draft: false
---

MIUI 系统会不定时的杀掉后台，罪魁祸首是 MiuiMemoryService。

杀后台的日志可以通过 logcat 查看：

```
logcat -b events | grep am_kill
```

通过 jadx-gui 反编译 service.jar framework.jar miui-service.jar miui-framework.jar 可以看到一些杀后台的逻辑，大部分可以通过 props 来控制。

## 添加下列 props

### mms 配置

mms 是 MiuiMemoryService 的缩写，这条是设置后台app数量上限，默认是100以下
```
persist.sys.mms.bg_apps_limit=1000
```

### 内存压力控制开关
```
persist.sys.spc.enabled=false
persist.sys.spc.extra_free_enable=false
persist.sys.spc.screenoff_kill_enable=false
```
### 禁用相机杀后台

修改 /system/system_ext/etc/camerabooster.json
字段 support.cam_boost_enable 设置为 false

如果没有的话看看 /odm/etc 下面没有

### 设置最大缓存进程数量

```
persist.device_config.activity_manager.max_cached_processes=1000
```

上述的 props 通过 magisk 模块修改即可。

也可以使用我制作好的模块: https://github.com/moonheart/moonheart.github.io/releases/download/miui-dont-kill-my-app/miui-dont-kill-my-app.1.2.zip