---
layout:       post
title:        "最后一个密码管理器－keepass"
subtitle:     ""
date:         2021-01-2
author:       "Benson"
header-img:   img/post-bg-20180108.jpg
header-mask:  0.3
catalog:      true
tags: 
    - keepass
	- 密码管理
---
# 最后一个密码管理器－keepass

Lastpass 用了 5 年，但自动输入持续出现问题，替换为自动输入超强的 Keepass。
官网下载：https://keepass.info/download.html
懒人包下载：
懒人包为绿色版，覆盖常用插件，无需设置直接使用。

## 配置

* 最小化到系统盘
* 关闭按钮最小化主窗口
* 自动输入规则修改`^{SPACE}{CLEARFIELD}{USERNAME}{TAB}{PASSWORD}{ENTER}`

  ^{SPACE} 即 Ctrl+Space，可关闭或启用当前输入法。关闭输入法后，默认为美式键盘输入。Ctrl+Space 需在输入法编辑状态下才能生效，而密码区都禁用输入法编辑。因此输入密码完成后，输入法有可能未启动，请按 Ctrl+Space 重新启用输入法。


## 必备插件
keepass 的主要功能都由插件实现，装了下列插件才能实现密码的

### [keepasshttp](https://github.com/pfn/keepasshttp/)
插件下载：https://raw.github.com/pfn/keepasshttp/master/KeePassHttp.plgx
下载后，将插件放入指定文件夹，重启 keepass 后生效。浏览器需搭配安排插件 KeePassHttp-Connector，达到自动输入密码效果。

### [keepassnatmsg](https://github.com/smorks/keepassnatmsg)
浏览器插件 KeePassHttp-Connector 已经不再更新。使用 keepassnatmsg 后，可连接 KeePassXC-Browser 连接浏览器。
> 搜狗浏览器不支持，只能继续使用 KeePassHttp-Connector。
>
> 如果报错「proxy download error」，备份并删除文件夹  C:\Users\%Username%\AppData\Local\KeePassNatMsg，然后重新加载 Native Messaging Host

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

## 常见问题
1. 一个密码能不能同时匹配多个链接？

	不能，但可以在「高级-标记 」上添加多个表格，用英文的逗号隔开。标记在全局匹配中可用于搜索。

2. 一个链接有多个账户密码，怎么默认第一个选择？

	不能默认第一个选择，但通过插件 keepasshttp、keepassnatmsg，按 username 或 title 排序。
	
3. Keepassxc 比 Keepass 界面更好，浏览器插件有原生支持，为什么不推荐 Keepassxc？

	Keepassxc 是 Keepass 的衍生版，Keepass 虽比 Keepassxc 复杂，但配置性更强。长期使用，个人推荐 Keepass 原版。

### 参考：

* [Keepass 教程之二——完美的通用自动输入规则](https://blog.csdn.net/SingWarm/article/details/90669580)