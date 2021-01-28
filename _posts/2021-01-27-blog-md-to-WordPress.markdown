---
layout:       post
title:        "Jekyll 博客迁移－从 markdown 到 WordPress"
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


![](http://tc.seoipo.com/20210127192533.png)

## 迁移步骤
这里迁移的是 Jekyll 的 [Hux blog 模板](https://github.com/Huxpro/huxpro.github.io)，Hexo 或其他博客也可以参考微调。
1. 修改`feed.xml`文件中的`{% for post in site.posts limit:100 %}`，该项为 rss最低生成量，我们导出所有文章，因此将该值修改为 100。如果 feed.xml 不存在，可尝试`rss.xml`或`atom.xml`。
2. 登录 WordPress 后台，工具－导入－安装并启用插件 **FeedWordPress** 。自带 RSS 导入器许久不更新，极易报错，不推荐。
3. 后台－Syndication－添加 rss 源如`xxx.com/feed.xml`，`xxx.com`为你的博客地址。然后导入`feed.xml`。
4. 删除 Syndicated Sites 并保存文章，如此你才能修改文章。

参考资料：
* [有没有办法把Markdown写的博客迁移到wordpress？](https://www.v2ex.com/t/73385)
