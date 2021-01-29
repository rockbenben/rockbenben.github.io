---
layout:       post
title:        "真·DPC_WATCHDOG_VIOLATION 蓝屏解决方案"
subtitle:     ""
date:         2020-10-25
author:       "Benson"
header-img:   img/post-bg-20180108.jpg
header-mask:  0.3
catalog:      true
tags:
    - 蓝屏
---
DPC_WATCHDOG_VIOLATION 多为硬件驱动出现问题。当驱动大规模报错时，厂家会更新并推送解决方法。所以，不要继续尝试其他人的解决方案，你的问题是独一无二的。分析 dmp 日志才是能百分百解决蓝屏的方法。

## 分析 dmg 日志
1. 安装 [WinDbg Preview](https://www.microsoft.com/zh-cn/p/windbg/9pgjgd53tn86?rtc=1&activetab=pivot:overviewtab)，这是微软官方推出的 Debug 工具。
2. 启动 WinDbg Preview，软件会自动检测到最新 dmp 日志，点击 Yes 即可载入。如果想分析其他文件，打开文件夹`C:\Windows\Minidump`，导出日志`xxx.dmp`。
3. 载入日志过程中，WinDbg Preview 会自动下载所需文件，不需要管。载入完成后，点击`!analyze -v`，分析具体日志。
![](http://tc.seoipo.com/20201025223307.png)
4. 分析日志：日志上方的都是套话，可以忽略。直接看最下方几行，找出引发蓝屏的进程，并删除该程序。如果冲突进程为驱动，无法强删，可以把驱动还原为上一版驱动或系统自带驱动。
![](http://tc.seoipo.com/20201025224308.png)

## 小白方案
不想分析日志，也简单。
1. 打开文件夹`C:\Windows\Minidump`，导出日志`xxx.dmp`。
2. 将 dmp 日志上传至[微软社区](https://answers.microsoft.com/zh-hans/newthread?threadtype=Questions&cancelurl=/zh-hans/windows/forum&forum=windows&filter=)，会有技术人员帮你分析蓝屏原因。微软社区无法上传附件，需先将 dmp 日志上传至百度云或微云，再将分享链接放在问题里。

**找不到 minidump 文件怎么办？**

尝试打开`%SystemRoot%\Minidump`，失败后按下列步骤修改：

1. 打开控制面板>>系统>>高级系统设置>>高级>>启动和故障恢复>>设置；
2. 写入调试信息>>选择「小内存转储（256KB）」，路径选择`%SystemRoot%\Minidump`，确定并重启您的计算机；
3. 再次异常关机后，前往`%SystemRoot%\Minidump`提取即可。

以下是网上流行的无效方法：SATA AHCI 驱动重置为系统自带驱动；更新显卡驱动；更新网卡驱动；擦拭内存条；重置 BIOS；增加 CPU 电压；关闭超线程；重装系统。(每个都试过了，不要尝试)