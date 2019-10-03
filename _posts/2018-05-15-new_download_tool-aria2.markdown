---
layout:       post
title:        "抛弃迅雷，Aria2 新手入门"
subtitle:     ""
date:         2018-5-15
author:       "Benson"
header-img:   img/post-bg-20180108.jpg
header-mask:  0.3
catalog:      true
tags:
    - Aria2
    - 下载工具
---
# 抛弃迅雷，Aria2 新手入门

迅雷已经用了 10 年，一直把它看作是速度最快也最方便的下载工具。迅雷会员也是我必续的服务。

但，**迅雷堕落了**。
* `thunder:\\` 迅雷专属链接越来越少，基本都是磁力、BT 的天下
* 迅雷会员加速不再给力，大量资源速度为0。会员虽然还有一年多，但几乎没用了
* 迅雷功能更新倒逼客户升级，而升到迅雷 U 享版后，迅雷提高自身权限，强制接收所有下载
> 有些小文件，我只想用浏览器直接下载。而一些不想下的链接，也会被迅雷非常敏感的感应到，强制下载，真的真的特别流氓。



##为什么选择 Aria2 ？

* 优点：比较全能，HTTP下载和BT下载都有，性能也相当不错，**速度不比迅雷慢**
* 缺点：无UI，需要自备；配置麻烦，上手较难。




## Aria2 快速配置

**1、下载[Windows 设置懒人包](http://aria2c.com/archiver/aria2.zip)**

**2、把懒人包解压到常用的存放目录，我放在 `D:\Aria2`**

**3、官网下载 [Aria2 程序](https://github.com/aria2/aria2/releases), 解压到懒人包目录中，会替代懒人包的 `aria2c.exe` 程序**

**4、点击 `aria2.exe` , 启动 aria2, 该程序会在任务栏中植入图标**

**5、修改`aria2.conf`，更多设置参考[Aria2 & YAAW 使用说明](http://aria2c.com/usage.html)**

* 修改默认下载目录

```
# 文件的保存路径(可使用绝对路径或相对路径), 默认: 当前启动位置
dir=D:\Download  #D:\Download 是我的默认下载目录
```

* 修改服务器默认连接数

```
# 同一服务器连接数, 添加时可指定, 默认:1
max-connection-per-server=16
```

* 开启 BT 下列设置

```
enable-dht=true
bt-enable-lpd=true
enable-peer-exchange=true
```

* 在最后添加 BT trackers, 配置列表时重新获取[最新 trackers](https://raw.githubusercontent.com/ngosang/trackerslist/master/trackers_best.txt), tracker 中用`，`隔开

```
# bt-tracker 更新，解决Aria2 BT下载速度慢没速度的问题
bt-tracker=udp://62.138.0.158:6969/announce,udp://87.233.192.220:6969/announce,udp://111.6.78.96:6969/announce,udp://90.179.64.91:1337/announce,udp://51.15.4.13:1337/announce,udp://151.80.120.113:2710/announce,udp://191.96.249.23:6969/announce,udp://35.187.36.248:1337/announce,udp://123.249.16.65:2710/announce,udp://210.244.71.25:6969/announce,udp://78.142.19.42:1337/announce,udp://173.254.219.72:6969/announce,udp://51.15.76.199:6969/announce,udp://51.15.40.114:80/announce,udp://91.212.150.191:3418/announce,udp://103.224.212.222:6969/announce,udp://5.79.83.194:6969/announce,udp://92.241.171.245:6969/announce,udp://5.79.209.57:6969/announce,udp://82.118.242.198:1337/announce
```

还有很多设置，有时间可以逐个修改。

**6、开始下载**

Aira2 没有软件界面，程序员可以用代码执行任务，但普通用户怎样添加下载任务呢？

打开浏览器，输入网址`aria2c.com`就可以打开操作界面了。可以把这个网址放到书签中，方便使用。



## Aria2 进阶

**1、使用不同的 Web UI**

AriaNg：https://github.com/mayswind/AriaNg

AriaNg 和传统下载软件界面类似，中文版界面，使用无压力，如果报错的话，记得修改服务器地址：

![](http://tc.seoipo.com/20180516104758.png)

使用方法：[下载zip包](https://github.com/mayswind/AriaNg-DailyBuild/archive/master.zip)，解压后直接运行index.html就可打开WebUI界面，可以收藏到书签，方便使用。

**2、Aira2 下载预热**

找个热门种子(千万建议是种子，而不是磁力链接)，然后下一波，挂着做种，过几个小时后退出Aria2，或者等Aria2会话自动保存，你会发现dht.dat从空文件变成有数据了，这时候你下载就会正常很多。

> 很多BT客户端一样，Aria2有个dht.dat文件(开启ipv6还有个dht6.dat)，这玩意用于存储一种叫做DHT Routing Table的东西，DHT网络由无数节点组成，你接触到一个后能通过它接触到更多的节点，Aria2我记得是有内置的节点，但是！如果你在Aria2第一次运行的时候直接下载磁力链接或者冷门种子，你很可能遇到连MetaData都无法获取的情况，这就是因为第一次只是初始化dht.dat文件，你本地不存在DHT Routing Table的缓存，所以你无法从DHT网络中获取足够的数据。

**3、安装 chrome 插件 - [添加到aria2](https://chrome.google.com/webstore/detail/nimeojfecmndgolmlmjghjmbpdkhhogl)，用 Aria2 接管 chrome 的下载**



## Aria2 启动器
Aria2 启动需要分别打开下载界面和 exe 应用文件，比较麻烦。我用 ahk 做了个启动器，可以检测 exe 应用运行状态并一键打开下载界面。
启动器下载：https://www.seoipo.com/software/Aria2_start.zip

将启动器到 Aria2 运行目录，如`D:\Aria2`。

**Aria2c启动器**：使用`http://aria2c.com/`作为默认下载界面，不需要多余设置。
**AriaNg启动器**：使用 AriaNg 作为下载界面，需将 AriaNg 解压到 Aria2 运行目录，如`D:\Aria2\AriaNg`。



参考资料：
1. [aria2 懒人安装教程](https://www.appinn.com/aria2-in-windows-setup/)
2. [Aria2+WebUI,迅雷倒下之后的代替品](http://blog.sina.com.cn/s/blog_6bf2cd8a0102x3w2.html)
3. [BT trackers 更新项目](https://github.com/ngosang/trackerslist)
4. [Aria2基础上手指南](https://zhuanlan.zhihu.com/p/30666881)
5. [解决Aria2 BT下载速度慢没速度的问题](http://www.senra.me/solutions-to-aria2-bt-metalink-download-slowly/)
6. [yaaw (国人开发的 Aria2 web-ui)]( https://github.com/binux/yaaw)
7. [bt-trackerlist 官方更新地址](https://github.com/ngosang/trackerslist)

