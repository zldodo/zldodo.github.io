---
title: 科学上网之路：搭建VPS+Shdowsocks+SwitchyOmega
date: 2017-09-11 15:07:07
tags:
- 工具
- VPS 
- Windows
categories:
- 探知
---
时代发展至今，自然环境已阻挡不了人们去寻觅世界的每一个角落。上山下海，无所不行。即便如此，却总有痴妄的人试图建立起一座隐形的高墙，去阻隔所谓的“外来腐化思想”。然而，思想的自由流通，理应不该受到这样的百般阻扰。于是，VPS为那些追求“独立之精神，自由之思想”理念的人开辟了一条绿色通道。

主流的VPS（虚拟主机）服务器提供商有三家：

- linode
- digital ocean
- bandwagon
针对个人用户，bandwagon是个不错选择，并且支持支付宝付款。


## 购买VPS服务
[购买链接](https://www.bwh1.net/)
[选型参考](http://www.laozuo.org/bandwagonhost)
付款成功后，查看你所购买的VPS服务端的详细信息，点击`KiwiVM Control Panel`
将会看到VPS登录用的IP和port信息。

然后，点击左侧的`Install new OS`,选择在Main controls中列出的Operating System 信号，例如 Centos 6 x86 bbr，即可获得root_password和port信息。

以上操作获得的IP，port和root_password将会在后面使用到。

## 配置服务端
下面，我们将远程登录到海外服务器，基于上面已经获取的IP，port和root_password的信息。

Window用户SSH登录需要[Xshell](http://www.netsarang.com/download/down_form.html?code=522&utm_source=textarea.com&utm_medium=textarea.com&utm_campaign=article)

### Xshell 登录 VPS
选择免费的Home&School user，下载，安装。然后按照Xshell使用的[官方教程](https://www.netsarang.com/tutorial/xshell/2776/Getting_Started?status=1&page=1&s_type=&s_text=)配置好IP和port。设置用户默认为root,注意这里要的password即是上面获得root_password。

登录成功后，命令行中将显示`[root@host ~]#`.

根据你购买的服务器端的操作系统，选择安装shadowsocks，可[参考链接](https://www.textarea.com/ExpectoPatronum/shiyong-shadowsocks-kexue-shangwang-265/)。
我的是CentOS, 所以安装命令是这样的

```
yum install python-setuptools && easy_install pip
pip install shadowsocks
```

如果`easy_install pip`找不到安装包，则使用以下办法：
```
wget https://bootstrap.pypa.io/get-pip.py
python get-pip.py
```

### 编写配置文件
下面配置shadowsocks启动参数文件。

shadowsocks启动时的参数，如服务器端口，代理端口，登录密码等，可以通过启动时的命令行参数来设定，也可以通过json格式的配置文件设定。推荐使用配置文件，方便查看和修改。

用vi新建一个配置文件：
```
vi /etc/shadowsocks.json
```
然后输入如下内容
```
{ 
   "server":"my_server_ip", 
   "server_port":"server_port", 
   "local_address": "127.0.0.1", 
   "local_port":1080, 
   "password":"mypassword",
   "timeout":300, 
   "method":"aes-256-cfb", 
   "fast_open": false
}
```
要是为了多个用户端使用，那就按下面的格式设置
```
{ 
   "server":"my_server_ip", 
   "port_password":{
   "server_port1":"password1",
   "server_port2":"password2",
   "server_port2":"password2"
   }
   "local_address": "127.0.0.1", 
   "local_port":1080, 
   "timeout":300, 
   "method":"aes-256-cfb", 
   "fast_open": false
}
```
### 启动shadowsocks
如果已经写好了配置文件，启动shadowsocks服务器的命令如下
```
ssserver -c /etc/shadowsocks.json -d start
ssserver -c /etc/shadowsocks.json -d stop
```
另外，要是后面又修改了配置文件，重新启动shadowsocks的命令是
```
ssserver -c /etc/shadowsocks.json -d restart
```

## 配置客户端

首先，需要下载客户端。Windows用户请移步[这里下载](https://github.com/shadowsocks/shadowsocks-windows/releases?utm_source=textarea.com&utm_medium=textarea.com&utm_campaign=article)，其他用户请参考[Github资源](https://github.com/shadowsocks)。

解压，然后运行.exe文件，在工具栏里出现“小飞机”图标，右击选择*服务器*->*编辑服务器*，出现一个窗口，填入已在服务端设置好的信息，my_server_ip，server_port，password。

然后右击“小飞机”图标，点击*启动系统代理*，就可以畅通无阻地愉快上网啦~

## 配置自动切换代理
** 为什么要配置自动切换代理？**

>Windows和macOS客户端支持全局代理和PAC代理两种方式，后者会使用一个脚本来自动检查一个网站是否在需要代理的网站列表中，自动选择直接连接或代理连接。

PAC列表可以在线更新，但是难免有收录不全的情况。如果你用Chrome,可以使用支持自定义规则的代理管理插件来实现自动切换代理，比如switchyOmega。

在Chrome应用商店里安装switchyOmega，然后进入switchyOmega的配置界面。

### 创建情景模式
新建一个情景模式，比如叫SS，代理协议选择socks5，代理地址为127.0.0.1，端口1080。

### 设置自动切换模式

在设置界面选择自动切换模式，在“切换规则”中勾选“规则列表规则”，对应的情景模式选择刚刚新建的SS。

然后在下面的规则列表地址中填写 
```
https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt
```
规则列表格式选择AutoProxy。这是一个一直在维护的被墙网站列表，项目地址是https://github.com/gfwlist/gfwlist。

然后点击立即更新情景模式， 更新完成后会有提示。

点击左侧的“应用选项”。然后单击switchyOmega图标，选择自动切换，就可以在访问“不存在的网站”时自动切换到shadowsocks代理了。

### 添加自定义规则

如果遇到某个国外网站无法直接连接或速度太慢时，可以单击switchyOmega图标，选择“添加条件”，情景模式选择SS，就可以了。

这时打开switchyOmega选项，在自动切换模式的切换规则中就可以看到刚刚添加的规则。可以在这里管理自定义的规则。

## 参考资料
这篇文档是站在巨人的肩上写成的。最后，附上巨人们写下的教程，作为大家参考。

【1】https://www.textarea.com/ExpectoPatronum/shiyong-shadowsocks-kexue-shangwang-265/
【2】http://blog.csdn.net/win_turn/article/details/51559867
【3】http://www.wuyuanhao.com/post/27 偷偷为师兄打个广告:)
