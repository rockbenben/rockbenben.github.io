---
layout:       post
title:        "小狼毫 3 分钟入门及进阶指南"
subtitle:     "终极输入法"
date:         2020-11-27
author:       "Benson"
header-img:   img/post-bg-20180108.jpg
header-mask:  0.3
catalog:      true
tags:
    - 小狼毫
    - 输入法
	- rime
---
# 小狼毫 3 分钟入门及进阶指南

常年使用搜狗输入法，备份时发现词库有 27 万条，99% 是垃圾词条，偶尔打过一次也被记录，而且无法从云端删除…… 这根本是个**键盘记录器**。

之后尝试各类输入法，百度、讯飞、手心等依旧是键盘记录器，影子输入法开源但不够稳定，谷歌拼音停止更新，微软拼音词库收录慢且难以转移。唯有小狼毫开源，且方便多设备同步词库。

小狼毫官网下载：https://dl.bintray.com/rime/weasel/weasel-0.14.3.0-installer.exe

国内搬运：https://www.lanzoux.com/ipfdfglhs8f

安装时建议不要修改用户文件夹位置，后续定制输入法容易出错。

安装完成后，右键点击任务栏小狼毫图标，选「输入法设定」，只勾选一个「朙月拼音·简化字」，再选一个喜欢的皮肤就好。

现在已经可以正常使用小狼毫输入法了。如果想要更完美的输入法，可以继续查看进阶指南。进阶前，右键点击任务栏小狼毫图标，选「用户文件夹」，新建 `luna_pinyin_simp.custom.yaml`(此方案为「朙月拼音·简化字」)。

## 进阶指南
官方文档：[定制指南](https://github.com/rime/home/wiki/CustomizationGuide)、[文件配置说明](https://github.com/rime/home/wiki/RimeWithSchemata#rime-%E4%B8%AD%E7%9A%84%E6%95%B8%E6%93%9A%E6%96%87%E4%BB%B6%E5%88%86%E4%BD%88%E5%8F%8A%E4%BD%9C%E7%94%A8)、[emoji 集成](https://github.com/rime/rime-emoji)、[模糊音设置](https://github.com/rime/home/wiki/CustomizationGuide#%E6%A8%A1%E7%B3%8A%E9%9F%B3)

扩充词库：[自定义短语](https://gist.github.com/lotem/5440677)、[Rime 擴充詞庫](https://github.com/rime-aca/dictionaries)

* [同步用户资料](https://github.com/rime/home/wiki/UserGuide#%E5%90%8C%E6%AD%A5%E7%94%A8%E6%88%B6%E8%B3%87%E6%96%99)：打开用户文件夹中的`installation.yaml`，将设备名称`installation_id`从长字符串修改为方便识别的名称，并在文件最下方添加`sync_dir: 'D:\Sync\RIME'`，此处为用户资料同步位置。注意：同步文件夹路径中不能出现中文。

如果要兼具英文联想、网络流行语、成语、俗语等，可使用下列词库
* BetterRime 词库：https://github.com/Chernfalin/better-rime-dict
* SuperRime 拓展词库：https://github.com/Chernfalin/SuperRimeDict

SuperRime 词库 > BetterRime 词库 > Rime 擴充詞庫，词库越大错误收录越多，按需选择词库。解压后，修改`luna_pinyin.extended.dict.yaml`，选择启用词库范围。`mysymbols.yaml`对全角和半角符号都做了优化，有问题的话可以按自己需求修改。

不建议安装「四叶草」等集成方案，而推荐以「朙月拼音·简化字」为基础定制你自己的输入法。这样即使出现 bug 也不会对你的输入法设置产生过大影响。小狼毫的魅力就在于定制、可控。

### 常见问题
* 开机后，输入法不能输出中文？
  需手动打开程序文件夹中的`WeaselServer.exe`即可，默认位置为`C:\Program Files (x86)\Rime\weasel-0.14.3\WeaselServer.exe`。不要手动将`WeaselServer.exe`设为开机启动，否则程序容易报错。
  
* 将用户文件夹置为同步文件夹，提示`有错误,请查看日志%TEMP%\rime.weasel.*.INFO`？

  不要将用户文件夹完整置为同步文件夹，会导致进程冲突，日志中有提示`另一个程序正在使用此文件，进程无法访问`。
* 打错了字，之后就总在前排出现，如何删除错误「上屏」的词？

   将选字光标移到要删除的词组上，再按下 Shift+Delete 或 Control+Delete。

* 官方文档中的`%APPDATA%\Rime`是用户文档吗？为什么有时位置不同？
  `%APPDATA%\Rime`是小狼毫默认的用户文档。如果在安装时修改了用户文档位置，右键点击任务栏小狼毫图标，选「用户文件夹」，会出现当前的位置，所有文档只需在这里修改。
  
* emoji 按教程设置，但始终无法显示？
 暂无解决方法。官方文档、三种集成词库都试过了，同样无法显示。特殊字符可使用 SuperRime 词库的 symbol 输出。

* SuperRime 词库安装后，无法完整触发特殊符号？
 SuperRime 词库自带的标点及特殊表情设置有问题。在`luna_pinyin_simp.custom.yaml`植入以下代码。
 ```yaml
 patch:
  #标点及特殊表情
  'punctuator/import_preset': mysymbols
  'recognizer/patterns/punct': "^/([a-z]+|[0-9])$"
 ```

### 参考资料
* [30分钟搞定 自由输入法RIME简明配置指南](https://www.jianshu.com/p/296bba666604)
* [小狼毫RIME输入法配置](https://www.dazhuanlan.com/2019/10/06/5d995d43e4432/)
* [Rime输入法—鼠须管(Squirrel)词库添加及配置](https://www.jianshu.com/p/cffc0ea094a7)
* [四叶草拼音输入方案](https://github.com/fkxxyz/rime-cloverpinyin)
* [小狼毫[rime_win][眀月拼音]简单配置方法](https://blog.csdn.net/qq_42204675/article/details/86422450)