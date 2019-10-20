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
* 自动输入规则修改`^+1{CLEARFIELD}{USERNAME}{TAB}{PASSWORD}%+0{ENTER}`
^+0即 Ctrl+Shift+1 切换为英文输入法、清空已有内容并自动输入用户名、自动输入密码、%+0 即 Alt+Shift+0 切换为中文输入法、 {ENTER} 按回车进入目标应用。该规则可以修复中文输入法无法正确输出问题，但需要安装纯英文输入法；然后设置中、英文输入法的激活热键，下图红框内第一个为英文输入法，第二个是常用的中文输入法：
![](http://tc.seoipo.com/20191016230117.png)
建议关闭「在输入语言之间」的按键顺序，防止语言切换出错。

## 进阶
使用中遇到几个问题
1. 一个密码能不能同时匹配多个链接？

  > 不能，但可以在「高级-标记 」上添加多个表格，用英文的逗号隔开。
  >
  > 标记在全局匹配中可用于搜索。

2. 一个链接有多个账户密码，怎么默认第一个选择？
> 不能默认第一个选择，但通过插件 keepasshttp、keepassnatmsg，按 username 或 title 排序。

## 必备插件

### [keepasshttp](https://github.com/pfn/keepasshttp/)
插件下载：https://raw.github.com/pfn/keepasshttp/master/KeePassHttp.plgx
下载后，将插件放入指定文件夹，重启 keepass 后生效。浏览器需搭配安排插件 KeePassHttp-Connector，达到自动输入密码效果。

### [keepassnatmsg](https://github.com/smorks/keepassnatmsg)
浏览器插件 KeePassHttp-Connector 已经不再更新。使用 keepassnatmsg 后，可连接 KeePassXC-Browser 连接浏览器。
> Chrome、Firefox 都支持该方法，但搜狗浏览器不支持，只能继续使用 KeePassHttp-Connector。

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

1. 点击 keepass 主界面的【文件】→【数据库设置】→【高级】，在【模板记录组】中选择一个群组→【确定】；
2. 返回主界面，点击步骤1中选择的群组，按 Ctrl+I 键（或点击上方工具栏的钥匙图标）添加记录；
3. 点击【自动输入】，勾选【双通道自动输入混淆】（**以后用模板添加记录时就不需要再勾选，一劳永逸**）；
4. 点击最左边的【Template】→【Init As Template】；
5. 配置所需模板→【确定】。

### [KPSourceForgeUpdateChecker](https://sourceforge.net/projects/kpsfupdatechecker/reviews)
keepass 菜单栏-帮助-检查更新，检查从 SourceForge 上的插件更新信息。

## 可选插件
### [KeeTrayTOTP](https://github.com/victor-rds/KeeTrayTOTP/releases/)
弹出的窗口中输入：
    初次创建二次验证时网站或应用提供的密钥（所以以前就创建好但没有保存好密钥的就要麻烦点重新生成两步验证了。）
    设置好验证码生效（动态生成）的时间，一般为30秒
    还有6位还是8位的动态码（一般为6位）
    最后一个修正时间的服务器URL可以留空不用填
https://www.cnblogs.com/tielemao/p/9613839.html

### [WebAutoType](https://keepass.info/plugins.html#webautotype)
WebAutoType 是很多人推荐的插件，其在热键后可以自动载入当前网址、标题。但对于已经有大量密码的人来说，并不实用，不推荐。

### [Yet Another Favicon Downloader](https://keepass.info/plugins.html#yafd)
自动匹配下载网站图标，但会巨幅增加数据库大小。对美观不是非常在意的话，不推荐 。

参考：

* [Keepass 教程之二——完美的通用自动输入规则](https://blog.csdn.net/SingWarm/article/details/90669580)