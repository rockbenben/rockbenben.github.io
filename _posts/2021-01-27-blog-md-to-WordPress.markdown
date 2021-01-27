---
layout:       post
title:        "jekyll 博客迁移－从 markdown 到 WordPress"
subtitle:     ""
date:         2021-01-27
author:       "Benson"
header-img:   img/post-bg-20180108.jpg
header-mask:  0.3
catalog:      true
tags: 
    - blog
    - WordPress
---
# jekyll 博客迁移－从 markdown 到 WordPress

2005 年开始在 MSN space 写博客，期间配合 Google Sidewiki(短命) 记录感想。2011 年 MSN space 关闭 ，博客被动转移到 WordPress 托管，个人也停止了博客记录。

2018 年接触 markdown 构成的 jekyll 博客，被其简洁的界面和便捷性打动，开始从感想记录转移到知识输出。本地 markdown 编辑，同步 github 即可实现博客修改发布。jekyll 博客用着太舒服，不知不觉就用了三年。但随着文章越来越多，修改也越来越困难，逐渐习惯只在知乎专栏上进行更新，博客却总未同步，无法起到知识记录参考的初衷。为了方便后续管理，开始将博客从 jekyll 迁移到 WordPress，后期 markdown 仅做初次排版编辑作用。

![](http://tc.seoipo.com/20210127192533.png)

### 迁移步骤
这里迁移的是 jekyll 的 [Hux blog 模板](https://github.com/Huxpro/huxpro.github.io)，Hexo 或其他博客也可以参考微调。
1. 修改`feed.xml`文件中的`{% for post in site.posts limit:100 %}`，该项为 rss最低生成量，我们导出所有文章，因此将该值修改为 100。
2. 将博客导出为 rss，如`xxx.com/feed.xml`，`xxx.com`为你的博客地址。如果 feed.xml 不存在，可尝试`rss.xml`或`atom.xml`。
3. 登录 WordPress 后台，工具－导入－安装 RSS 导入器，然后导入`feed.xml`。

参考资料：
* [有没有办法把Markdown写的博客迁移到wordpress？](https://www.v2ex.com/t/73385)
