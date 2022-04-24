---
title: 在 aspnet core 中使用类似 javaagent 的方式注入程序集
date: 2022-02-14T10:31:23+08:00
draft: false
---

> https://docs.microsoft.com/en-us/aspnet/core/fundamentals/host/platform-specific-configuration?view=aspnetcore-6.0#activation

通过这种方式, 可以实现不对原有项目做任何更改, 可以直接对已发布好的程序文件进行自定义程序集注入. 可以使用在 k8s 的 init-container 中注入统一的日志, 配置, 健康检查, 服务注册, 等等.

需要配置三个关键的环境变量:
- 使用 ASPNETCORE_HOSTINGSTARTUPASSEMBLIES 指定要使用的程序集
- 使用 DOTNET_ADDITIONAL_DEPS 指定要添加的依赖
- 使用 DOTNET_SHARED_STORE 来指定依赖查找的目录

另一个额外注意点是, 我们注入的程序集需要支持主程序的版本. 