---
layout:       post
title:        "RSS 速成篇2：RSSHub 自部署"
subtitle:     ""
date:         2020-3-25
author:       "Benson"
header-img:   img/post-bg-20180108.jpg
header-mask:  0.3
catalog:      true
tags:
    - rss
    - RSSHub
---
RSSHub 使用非常简单，但随着使用者增多，微博、知乎加大了反爬限制。目前大量源都无法直接使用，只能自建 RSSHub 来解决稳定性。部署前，准备好**域名和服务器**。

新手使用 RSSHub 部署教程报错较多，本篇将使用宝塔 PM2 管理器进行部署。

## 部署步骤

1. 将 rsshub 代码下载到根目录 /root/
```
git clone https://github.com/DIYgod/RSSHub.git
```
2. 安装宝塔面板，查看[官方安装教程](https://www.bt.cn/bbs/thread-19376-1-1.html)
3. 登陆宝塔面板，点击「软件商店」-「运行环境」，找到并安装**PM2 管理器**
   ![](http://tc.seoipo.com/20200325120705.png)

4. 点击 PM2 管理器右侧的设置，按图中红字添加项目路径，启动文件名称为 lib

![](http://tc.seoipo.com/20200325121639.png)

添加后，点击项目中的「映射」，输入指定域名，如 rsshub.xxx.com ，服务器的 1200 端口将指向该域名。映射域名需解析到服务器 IP。

![](http://tc.seoipo.com/20200325121921.png)

程序运行，有时会报错，也可以直接用命令运行 pm2 `pm2 start /root/RSSHub/lib/index.js --name rsshub`。



## 使用步骤

1. 打开 [RSSHub 接口指南](https://docs.rsshub.app/)，搜索需要订阅的网站。RSSHub 支持国内大部分的主流网站。

2. 根据 [bilibili 番剧路由](https://docs.rsshub.app/social-media.html#bilibili)的文档，将生成源  `https://rsshub.app/bilibili/bangumi/media/9192` 其中域名 `https://rsshub.app` 替换为你自部署的域名，如`http://rsshub.xxx.com/bilibili/bangumi/media/9192`。

   另外 RSSHub 支持很多实用的参数，比如内容过滤、全文输出等。全文输出参数为`mode=fulltext`，应用举例: 果壳科学人全文输出 `https://rsshub.xxx.com/guokr/scientific?mode=fulltext`。其他可以在 [通用参数](https://docs.rsshub.app/parameter.html) 官方文档了解具体使用方法。

**RSSHub 和 Huginn 的区别：**

* RSSHub 使用简单，使用现成的抓取规则，适用于国内主流网站；但无法抓取对小众网站，必须 RSSHub 官方定制订阅源。
* Huginn 适用于所有网站，可设定抓取频率、内容结构、js结果、输出样式等；但部署、配置复杂，入门门槛高，需要针对网站单独定制抓取规则。