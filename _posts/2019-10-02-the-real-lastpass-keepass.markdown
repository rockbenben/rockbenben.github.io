---
layout:       post
title:        "替代 Lastpass - keepass"
subtitle:     ""
date:         2019-10-2
author:       "Benson"
header-img:   img/post-bg-20180108.jpg
header-mask:  0.3
catalog:      true
tags: 
    - keepass
---
# 替代 Lastpass - keepass

Lastpass 自动输入功能持续变弱，决定加入自动输入超强的 Keepass。Keepass 原版比 keepassxc 复杂，但配置性更强。

## 配置

* 最小化到系统盘
* 关闭按钮最小化主窗口

## 插件

### [keepasshttp](https://github.com/pfn/keepasshttp/)
插件下载：https://raw.github.com/pfn/keepasshttp/master/KeePassHttp.plgx
下载后，将插件放入指定文件夹，重启 keepass 后生效。

### [KPEnhancedEntryView](https://keepass.info/plugins.html#kpenhentryview)
KPEnhancedEntryView 增强视觉效果，为达到最佳显示效果，按以下配置：
* 在主界面中点击【显示】→【窗口布局】→【平铺】；
* 在主界面中点击【显示】→【列设置】，只选择：标题→【确定】。大家也可以按自己要求选择。

### [AutoTypeSearch](https://keepass.info/plugins.html#atsearch)
AutoTypeSearch 提供全局框，输入热键`Ctrl+Shift+A`后，搜索关键词输入密码
配置图：
![](http://tc.seoipo.com/20191013083950.png)

### [KP Entry Templates](https://github.com/mitchcapper/KPEntryTemplates)

配置方法：

1. 点击keepass主界面的【文件】→【数据库设置】→【高级】，在【模板记录组】中选择一个群组→【确定】；
2. 返回主界面，点击步骤1中选择的群组，按Ctrl+I键（或点击上方工具栏的钥匙图标）添加记录；
3. 点击【自动输入】，勾选【双通道自动输入混淆】（**以后用模板添加记录时就不需要再勾选，一劳永逸**）；
4. 点击最左边的【Template】→【Init As Template】；
5. 配置所需模板→【确定】。