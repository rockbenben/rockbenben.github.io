---
layout:       post
title:        "放弃 GitHub Page,在 VPS 上部署 Jekyll"
subtitle:     ""
date:         2018-4-30
author:       "Benson"
header-img:   img/post-bg-20180108.jpg
header-mask:  0.3
catalog:      true
tags:
    - Jekyll
---
# 放弃 GitHub Page,在 VPS 上部署 Jekyll

一直都想建立自己的个人博客，重装过 N 次 WordPress，又因为种种原因而放弃。最后选择了有众多 Jekyll

Jekyll 和 Github 已经被锁定在一起，空间免费、流量免费、部署方面。
但 Github 属于被墙状态，将博客部署在那，速度实在太慢。

我这介绍的是传统安装Jekyll
1. 使用 RVM 安装 Ruby (网上大量教程就不赘述了)
2. ​
```
gem install jekyll
```
3. 自动生成 Jekyll 网页
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
openssl aes-256-cbc -K $encrypted_0a6446eb3ae3_key -iv $encrypted_0a6446eb3ae3_iv -in super_secret.txt.enc -out super_secret.txt -d

ssh root@106.15.190.249:27378 "docker restart mritd_jekyll_1"
```

```
# travis encrypt-file ~/.ssh/id_rsa --add -r rockbenben/rockbenben.github.io
```



网站建设用宝塔面板





## Travis CI 通知服务器

1. 生成公钥/私钥对 （Git 用户可省略，已经生成过了）
2. **将生成的公钥添加为受信列表**
3. 在 Linux 服务器安装 Travis 客户端 （rvm -> ruby -> gem ->Travis）
4. 服务器登录 Travis, 添加加密的私钥至代码仓库
5. Travis  免密码 ssh 登录服务器，执行命令行`git pull`

```
language: ruby
rvm:
- 2.3.3

before_script:
- openssl aes-256-cbc -K $encrypted_ecabfac08d8e_key -iv $encrypted_ecabfac08d8e_iv
    -in id_rsa.enc -out ~/.ssh/id_rsa -d
  
# Assume bundler is being used, therefore
# the `install` step will run `bundle install` by default.
install: 
- gem install jekyll
- gem install jekyll-paginate

script: jekyll build #&& htmlproofer ./_site

after_success:
  - git clone https://github.com/rockbenben/rockbenben.github.io.git
  - cd rockbenben.github.io && rm -rf * && cp -r ../_site/* .
  - git config user.name "rockbenben"
  - git config user.email "qingwhat@gmail.com"
  - git add --all .
  - git commit -m "Travis CI Auto Builder"
  - git push --force https://$DEPLOY_TOKEN@github.com/rockbenben/blog.git master
  - ssh root@106.15.190.249 -o StrictHostKeyChecking=no 'cd ~/blog-front && git pull && npm install && npm run build'   #使用ssh连接服务器

# branch whitelist, only for GitHub Pages
branchs:
  only:
  - master  #指定只有检测到master分支有变动时才执行任务

env:
  global:
  - NOKOGIRI_USE_SYSTEM_LIBRARIES=true # speeds up installation of html-proofer

addons:
  ssh_known_hosts:
  - 106.15.190.249 #受信主机，你的Linux服务器ip

sudo: false # route your build to the container-based infrastructure for a faster build
```



Dockerfile

样例 Dockerfile: https://github.com/mritd/dockerfile/tree/master/mritd

```
FROM nginx:1.13.9-alpine

LABEL maintainer="Benson <qingwhat@gmail.com>"

ARG TZ='Asia/Shanghai'

ENV TZ ${TZ}

RUN apk upgrade --update \
    && apk add bash git \
    && rm -rf /usr/share/nginx/html \
    && git clone https://github.com/rockbenben/blog.git /usr/share/nginx/html \
    && ln -sf /usr/share/zoneinfo/${TZ} /etc/localtime \
    && echo ${TZ} > /etc/timezone \
    && rm -rf /var/cache/apk/*

ADD entrypoint.sh /entrypoint.sh
ADD flush /usr/local/bin/flush

WORKDIR /usr/share/nginx/html

CMD ["/entrypoint.sh"]
```





参考资料：
[Jekyll + Travis CI 自动化部署博客](https://mritd.me/2017/02/25/jekyll-blog-+-travis-ci-auto-deploy/)
[如何将dockerhub与github关联](https://blog.csdn.net/yinweitao12/article/details/73165914)