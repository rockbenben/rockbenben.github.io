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

# 新手入门 Jekyll

https://github.com/Huxpro/huxpro.github.io



我这介绍的是传统安装Jekyll
1. 使用 RVM 安装 Ruby (网上大量教程就不赘述了)
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



# Docker 镜像

建立 Automated Build 镜像，然后在服务器上执行 `docker pull rockben/jekyll` , 镜像会存储在默认 docker 文件上 `/var/lib/docker`



**创建并运行容器**：

`docker run --name=jekyll_blog -d -p 39100:80 -v /www/wwwroot/jekyll:/jekyll --privileged=true rockben/jekyll:latest /bin/bash`

--name=jekyll_blog 中的 `jekyll_blog`是对容器的命名，方便后续操作

-d 让容器在后台运行。

-p 映射端口: 80 是容器内对应的端口，39100 是主机端口，也就是最终用户访问的端口，本端口可以自由选择。

-v 挂载目录/root/software 本地目录 /software容器目录，在创建前容器是没有software目录的，docker 容器会自己创建

--privileged=true 关闭安全权限，否则你容器操作文件夹没有权限

--`rockben/jekyll:latest  `容器名称, 可省略 `:latest`

--`/bin/bash` 这是 CMD 命令行，可不填



**最终命令行**：

```
docker run --name=jekyll_blog -d -p 39100:80 --privileged=true rockben/jekyll
```

运行容器后，访问 `seoipo.com:39100 `就可以看到镜像网页。如果不想用端口访问，可以在域名 DNS 中设置隐形URL，将二级域名 `blog.seoipo.com` 指向 `seoipo.com:39100`



## Travis CI 通知服务器

1. 生成公钥/私钥对 （Git 用户可省略，已经生成过了）
2. **将生成的公钥添加为受信列表**
3. 在 Linux 服务器安装 Travis 客户端 （rvm -> ruby -> gem ->Travis）
4. 服务器登录 Travis, 添加加密的私钥至代码仓库
5. Travis  免密码 ssh 登录服务器，执行命令行重启 docker

```
language: ruby
rvm:
- 2.3.3

before_script:
- openssl aes-256-cbc -K $encrypted_5c280379e96c_key -iv $encrypted_5c280379e96c_iv
  -in id_rsa.enc -out ~/.ssh/id_rsa -d      #本句是服务器上的 Travis 自动生成的，但默认生成的命令可能会在/前面带转义符\，我们不需要这些转义符，手动删掉所有的转义符，否则可能在后面引发莫名的错误。
- chmod 600 ~/.ssh/id_rsa
- echo -e "Host 106.15.190.249\n\tStrictHostKeyChecking no\n" >> ~/.ssh/config

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
  - ssh root@106.15.190.249 -p 27378 -o StrictHostKeyChecking=no  "docker restart jekyll_blog"  #ssh 连接后，重启 docker 容器, jekyll_blog 为之前设定的容器名。
  # -p 27378 是我自设的服务器端口，默认是22
  #- ssh root@106.15.190.249 -o StrictHostKeyChecking=no 'cd ~/blog-front && git pull && npm install && npm run build'   #使用ssh连接服务器, git pull?

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



# 添加加密的私钥至代码仓库

在服务器端建空白 .
```
travis encrypt-file ~/.ssh/id_rsa --add -r rockbenben/rockbenben.github.io
```







# SSH 免密登录服务器

将生成的公钥添加为受信列表（重点）
```
[travis@VM_156_69_centos ~]$ cd .ssh/
#将公钥内容输出到authorized_keys中
[travis@VM_156_69_centos .ssh]$ cat id_rsa.pub >> authorized_keys
[travis@VM_156_69_centos .ssh]$ cat authorized_keys 
# authorized_keys文件内容类似这样
ssh-rsa  *************centos
```





# Dockerfile

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

# ADD entrypoint.sh /entrypoint.sh #容易报错
# ADD flush /usr/local/bin/flush #容易报错

WORKDIR /usr/share/nginx/html

# CMD ["/entrypoint.sh"] #容易报错
```



参考资料：

[Jekyll 模板 hux blog](https://github.com/Huxpro/huxpro.github.io)

[一点都不高大上，手把手教你使用Travis CI实现持续部署](https://zhuanlan.zhihu.com/p/25066056)

[Jekyll + Travis CI 自动化部署博客](https://mritd.me/2017/02/25/jekyll-blog-+-travis-ci-auto-deploy/)

[Travis-CI自动化测试并部署至自己的CentOS服务器](https://juejin.im/post/5a9e1a5751882555712bd8e1)

[Travis CI 系列：自动化部署博客](https://segmentfault.com/a/1190000011218410)

[SSH 免密登录远程服务器](https://juejin.im/post/5a2941ad6fb9a045030ffc95)

[SSH公钥登录原理](http://www.cnblogs.com/scofi/p/6617394.html)

[如何将dockerhub与github关联](https://blog.csdn.net/yinweitao12/article/details/73165914)

[docker 启动，端口映射，挂载本地目录](http://www.cnblogs.com/YasinXiao/p/7736075.html)

[Docker — 从入门到实践](https://yeasy.gitbooks.io/docker_practice/)