---
layout:       post
title:        "scrcpy - 手机无线投屏到电脑"
subtitle:     ""
date:         2019-08-26
author:       "Benson"
header-img:   img/post-bg-20180108.jpg
header-mask:  0.3
catalog:      true
tags:
    - scrcpy
---
**scrcpy** 是免费开源的投屏软件，支持将安卓手机屏幕投放在 Windows、macOS、GNU/Linux 上，并可直接借助鼠标在投屏窗口中进行交互和录制。

项目地址：https://github.com/Genymobile/scrcpy

Windows 下载：[`scrcpy-win64-v1.12.zip`](https://github.com/Genymobile/scrcpy/releases/download/v1.12/scrcpy-win64-v1.12.zip)

电脑端完成配置后，我们还需要在手机端开启 `开发者选项` 及 `USB 调试`。然后使用数据线将手机和电脑连接并允许 USB 调试，即可双击解压得到的 **scrcpy.exe** 文件进行有线投屏了。

### 无线投屏 (WIN 10)
1. 确保 PC 和手机处于同一局域网中
2. 打开 PowerShell (~ cmd)，依次操作并输入代码
```
# a.将代码目录定位到 scrcpy 文件夹
cd D:\Libraries\Desktop\scrcpy-win64-v1.12

# b.在手机端开启「开发者选项」及「USB 调试」，然后使用数据线将手机和电脑连接并允许 USB 调试，开启手机端口
.\adb tcpip 5555

# c.拔出手机数据线，开启无线链接。(192.168.2.234 为手机端 ip，需更改)
.\adb connect 192.168.2.234:5555

# d.启动 scrcpy.exe
.\scrcpy
```
![](http://tc.seoipo.com/20190829093407.png)

### 屏幕录制

打开 PowerShell (~ cmd)，依次操作并输入代码
```
# 将代码目录定位到 scrcpy 文件夹
cd D:\Libraries\Desktop\scrcpy-win64-v1.12

# 开始录制，录屏文件会以命令指定的文件名自动保存在当前文件夹内。
.\scrcpy -r filename.mp4

# 关闭投屏窗口后，自动停止录屏并将视频保存在相应目录
```

### 解决投屏模糊

如果屏幕设置了缩放比例，投屏界面会模糊。右键 **scrcpy.exe**，属性 - 兼容性 - 更改高 DPI 设置 - 勾选替代高 DPI 缩放行为，应用后，该问题可解决。

![](http://tc.seoipo.com/20190829095640.png)