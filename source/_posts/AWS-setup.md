---
title: 搭建AWS云计算配置环境
date: 2017-09-05 16:10:02
tags:
    - GPU
    - Deep Learning
    - Windows
categories:
    - 探知
---


实操是快速有效掌握一门知识的最好途径。随着AI崛起，许多科技热点不断涌现，机器学习当属个中翘楚。想要愉快玩耍机器学习，自然离不开GPU的支持。但对于新手入门，想要自己搭建GPU服务器，工程量显得有些大；再加上囊中羞涩，个人购买GPU实在有些困难。在这种情况下，云计算服务是一个不错的选择。

# 什么是云计算？
简单说来，云计算就是通过 Internet（“云”）交付服务器、存储空间、数据库、网络、软件和分析等计算服务。提供这些计算服务的公司称为云提供商，他们通常基于用户使用对云计算服务进行收费，类似于家用水电的计费方式。

# 云计算服务提供商
- AWS 
亚马逊出品，市场老大，商业云计算名义上的先驱，主要针对企业级用户，特别是在国内没有开放个人账户。

- Azure 
微软出品，与自家产品无线缝合，一条龙服务，正在追赶AWS。

-  APP engine 
谷歌出品，大名鼎鼎Mapduce框架的提出者，在分布式计算和分布式存储方面掌握核心科技。

- OVH 
一家法国公司，价格比较低，性价比高。

- 阿里云 
阿里巴巴出品，在全球来看算是第二梯队的厂商，拥有本土优势。

# AWS环境搭建

最近正在学习一门MOOC课程，叫[Practical Deep Learning for Coders](http://course.fast.ai/index.html), 由 JEREMY HOWARD （President Kaggle ）讲授，AWS是这门课推荐，所以我也就一个一个轮子地搭建AWS GPU server。

首先，推荐观看这个[新手指南视频](https://www.youtube.com/watch?v=8rjRfW4JM2I),配套有[wiki page](http://wiki.fast.ai/index.php/AWS_install),里面有常见问题的解决办法。如果有些问题wiki页面里没有涉及，你可以在这个[论坛](http://forums.fast.ai/)查找和询问。最终，你一定能得到解决办法的。

AWS GPU server的搭建过程我不祥述，上面的新手指南和wiki已经说明得十分详细，下面主要罗列出我在搭建环境中遇到问题及解决方法，方便大家使用。

## Errors: bash setup_p2.sh or setup_t2.sh
我在这儿卡壳了很久，遇到了许多奇奇怪怪的错误。而且，我也发现这部分是论坛里面大家问的最多的地方。
{% codeblock %}
    setup_t2.sh: line 7: syntax error near unexpected token `newline'
    setup_t2.sh: line 7: `<!DOCTYPE html>'
{% endcodeblock %}
setup_t2.sh 格式问题，不应是HTML格式 

```
    '\r': command not found
```
setup_t2.sh 格式问题，用notepad++z转换成UNIX格式

后面发现，这两个问题同属同一个原因：当使用`wget url`时，复制的地址下载的是html 格式文件，而不是我们需要的.sh或者.py格式。解决办法是在github里clone所需文件，然后复制到cygwin下面的目录。另外，我还发现了一个办法，从论坛[这里](http://forums.fast.ai/t/unreadable-notebook-home-ubuntu-nbs-lesson1-ipynb-notjsonerror-notebook-does-not-appear-to-be-json/2966/4)搬运来的。具体是在github中选择文件的 view-raw模式，然后复制地址，就可以使用`wget`的方式下载文件了。

## Error ：ssh command not found
需要安装一下openssh
``` 
    apt-cyg install openssh
```

## Start Over with AWS步骤
若遇到实在无法解决的错误，可以考虑重新安装一次，参考[Start Over with AWS](http://wiki.fast.ai/index.php/Starting_Over_with_AWS)
** 切记**！！！要删除.pem,通过`rm ~/.ssh/aws-key-fast-ai.pem`。

## Error occurred (AddressLimitExceeded)  
这是因为Elastic IP或者VPCs超过指定数量，我测试了下，似乎不允许炒过5个。
release Elastic IP和delete your VPCs 的步骤见[Start Over with AWS](http://wiki.fast.ai/index.php/Starting_Over_with_AWS)。

** Tips:每次bash前，需关闭所有Instance **


## 启动AWS
```
    source aws-alias.sh
    alias
    aws-get-p2
    aws-start
    aws-ssh
```
可将`source aws-alias.sh`写入`vim .bashrc`。
若出现`vim command not found`，这是因为缺少vim安装包。重新启动cygwin的setup.exe，默认安装，直到选择packages。选择 vim安装包，下一步安装。

## vim 使用
```
i 插入模式
esc 命令模式
:q 退出
:wq 保存退出
```

** 走到这一步，AWS配置就基本完成啦**

# 好用的终端小工具

tmux是linux中一种管理窗口的程序, 它提供了一个Session随时存储和恢复的功能, detach Session(保持Session后台运行)然后重新attach Session。

>常用场景, 在公司Terimal中开了多个标签和文件, 下班回家忽然有了灵感想要继续编写, 使用ssh远程链接公司电脑, 然后发现标签页和文件都要重新打开, 如果使用Tmux, 下班了detach当前Session, 回家ssh远程连接后, attach Session后, 场景恢复又能愉快的继续编程了…

## tmux使用

前缀键`Crtl+b`
按过前缀键后，然后按一下c =创建新窗口
按过前缀键后，然后按一下% =水平分割
按过前缀键后，然后按一下" =垂直分割

基于前缀键，还有常用的其他命令：
```   
    o swap panes
    q show pane numbers
    x kill pane
```
** 如有任何AWS配置安装问题，欢迎在评论中交流哈~**

