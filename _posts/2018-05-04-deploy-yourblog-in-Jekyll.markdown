---
layout:       post
title:        "Jekyll 入门：服务器搭建"
subtitle:     ""
date:         2018-5-4
author:       "Benson"
header-img:   img/post-bg-20180108.jpg
header-mask:  0.3
catalog:      true
tags:
    - 建站
    - Jekyll
---
# Jekyll 入门：服务器搭建

## 新手入门 Jekyll

服务器上安装 Jekyll 环境，参考官方安装指南：https://www.jekyll.com.cn/docs/quickstart/
1. 使用 RVM 安装 Ruby (rvm -> ruby -> gem 网上大量教程就不赘述了)
2. 安装 jekyll 
```
gem install jekyll
```
3. 进入 jekyll 网站，执行命令行，生成 Jekyll 静态网页
```
cd /www/wwwroot/blog
jekyll build  #生成静态页面，否则无法显示最新内容
```
4. 网站的执行目录需要指定在 \_site，这是  Jekyll 生成的静态页面目录。
5. 设置定期生成代码，每隔 15 分钟生成一次？
  Travis CI ：能自动感知到 Github 的 commit；所以自动 build 生成 静态文件


### 解决思路
* 本地提交博客 Markdown 文件 到 Github
* Github 触发 Travis CI 执行自动编译
* Travis CI 编译后 push 静态文件到 Github

* Travis CI 通知服务器重启博客容器
* 容器重启拉取最新静态文件完成更新

```
# travis encrypt-file ~/.ssh/id_rsa --add -r rockbenben/rockbenben.github.io
```

