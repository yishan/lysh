---
layout: post
title: 如何正常上网
---

>   VPN供应商Astrill本周通知用户，由于防火长城的升级，使用IPSec、L2TP/IPSec和PPTP协议的设备无法访问它的服务。受影响的主要是iOS设备，其它使用不同协议的设备仍然能正常工作。

>   另一家VPN服务商VPN Tech Runo本月早些时候称，从去年12月31日开始它的很多IP地址已被屏蔽，部分地区使用L2TP协议的用户也连接不了它的服务器。

>   中国工信部此前发布规定，在中国提供VPN服务的公司必须登记注册，未登记的VPN服务商将“不会受到中国法律的保护”。

>   —— [中国屏蔽外国VPN服务](http://www.solidot.org/story?sid=42796) - Solidot.org

早上看到这么一条消息，如果属实，那么除 Astrill 之外其它我用过以及正在使用的 VPN 都存在无法使用的可能。事实上，除了 IPSec、L2TP 和 PPTP 之外，我还都没使用过其它协议的透墙技术。再想到这几天 Goagent 满屏幕的链接超时，基本陷入瘫痪状态。

还好，几天前经人推荐，知道了 Shadowsocks 这个东西。

在某个闲暇的凌晨跳转于不同标签之前查看资料捣腾好之后，到今天已经用了近100个小时，感觉还不错。虽然现在看官方帮助文档还有些不甚明白的地方，但弄出一个可用的工具没什么问题。接下来简单说下步骤，希望大家都有网上，也上好网。

## VPS

VPS(virtual Private Server)，虚拟专用服务器。租赁一个 VPS 是搭建 Shadowsocks 或者 VPN 的前提，此外也可以用来搭建网站等等。

VPS 的选择挺多，查资料时发现，因为挺多 VPS 有推荐注册返利的活动设置，所以大多数教程提供的 VPS 都集中在那么几家，比如常看到的 [Linode](https://www.linode.com/) 和 [DigitalOcean](https://www.digitalocean.com/?refcode=08c5123baa50)（此处有返利链接），我选用了后者，费用较低，适合摸索试用。

以下均以 DigitalOcean 为例。

## 设置 VPS

VPS 相对虚拟空间权限多，定制空间高，需要自己干的活也不少。

DigitalOcean 无试用，注册后初始需要支付 5 美刀（应该算充值到账户，会根据使用时间扣除费用），推荐使用 Paypal 进行支付，支付方式可行且方便。

之后可以在账户下 `Create Droplet` 创建一个 VPS，推荐最便宜的一档，5刀/每月。这里的 Droplet Hostname 随意，方便自己记忆就可以。机房位置_建议选择 San Francisco_，大部分评论都反馈 SF 的机房响应速度快，而看上去应该更快的新加坡机房反而访问不是太流畅。

![DO Create Droplet](\images\2015_01\do_create_droplet.png =600x)

![DO Select Region](\images\2015_01\do_select_region.png =600x)

接下来选择操作系统，这里选择的是 Ubuntu 12.04.5 x32（和另一个搭建 VPN 的教程有关，同样也可以用来搭建 Shadowsocks)。

![DO Select Image](\images\2015_01\do_select_image.png =600x)

最后点击页面下方的 `Create Droplet` 就可以了。

稍后会收到一封官方邮件，提示 Droplet 已建好，同时附有 IP 地址、管理帐号及帐号密码。

![DO Mail](\images\2015_01\do_mail_check.png =600x)

同个 DigitalOcean 帐号下可以同时建立 10 个 Droplet，所以以上关于系统的选择可以多尝试几个，比如多数教程所采用的 CentOS + Shadowsocks 的组合。

## 配置 Shadowsocks

### 登录 VPS

对 VPS进行操作需要远程登录服务器进行操作，有两种方法。

1.  在 DigitalOcean 后台进行操作。选择图中的 Console Access 后会在页面上出现一个黑色命令行区域，单击即可进行操作。

![DO Console Access](\images\2015_01\do_console_access.png =600x)

2.  使用 SSH 工具连接到服务器，我使用的是 [PuTTY](the.earth.li/~sgtatham/putty/0.63/x86/)，软件界面如下图所示，填入邮件里发来的 IP 地址（可在橙色框内填入备注并保存，下次链接可无需重复输入）后点击 `Open` 打开命令行窗口，并连接至服务器。

![PuTTY](\images\2015_01\putty.png =600x)

第一种方法因为在 DigitalOcean 后台操作，所以已经有了操作权限，可直接跳到下一步。

第二种方法需要使用邮件给的 root 帐号登录一下才可以使用，此时窗口会提示

    login as:
    
输入 `root` 回车，出现如下字样

    root@(IP地址)'s password:
    
输入邮件里的密码，回车登录（此时屏幕不会显示任何字符，敲键盘时多加注意是否输入正确）

首次登陆后服务器会要求即刻更改登录密码，依次键入并回车分配密码，新密码，新密码后，下一次通过 SSH 登录时应使用新密码登录。

    (current) UNIX password:
    Enter new UNIX password:
    REtype new UNIX password:
    
搞定，接下来是 Shadowsocks 的安装。

### 安装

Shadowsocks 提供了[多种部署方式](http://shadowsocks.cn/servers.html)，我选用的是 Python，官方步骤直接使用 pip 这个工具来安装 Shadowsocks，但我们的 VPS 里得先安装 pip，所以先输入

    sudo apt-get install python-pip
    
然后

    sudo pip install shadowsocks

等待安装完成即可。

然后是进行参数配置，输入如下命令创建配置文档

    sudo nano /etc/shadowsocks.json

复制以下内容，在窗口内右键直接粘贴

    {
    "server":"my_server_ip",
    "server_port":8388,
    "local_port":1080,
    "password":"barfoo!",
    "timeout":300,
    "method":"aes-256-cfb"
    "fast_open": false,
    }
    
>   说明："server" 后填入 IP，"server_port" 端口可选填大于 1024、小于 65535 的数字，也可以保留原始的 8388，密码更改为自己好记的内容，后面客户端需要用到。

更改好之后 Ctrl X 保存并退出编辑，会询问是否保存，输入 `Y`，回车。

最后启动 Shadowsocks

    ssserver -c /etc/shadowsocks.json -d start
    
此时 Shadowssocks 会在后台运行，需要停止运行则输入

    ssserver -c /etc/shadowsocks.json -d stop

到这里服务器端就配置完成了。

## 客户端

[Shadowsocks 客户端](http://shadowsocks.cn/clients.html)基本覆盖全平台，我在用着的几个操作都挺方便，配置也大多是修改下 IP 地址、端口和密码就可以使用。

### Windows

Windows 下分有图形化界面客户端和命令行客户端两种，这里使用 `shadowsocks-gui: 0.6.2-win-ia32.tar.xz` 的版本，下载到本地后解压出 `Shadowsocks.exe`，运行后客户端将加载至系统通知栏。

右键通知栏图标

-   选择 `服务器 > 编辑服务器`，在下图所示窗口中填入 IP 地址、端口以及加密方式，保存。
-   `代理模式` 内可选 `PAC` 和 `全局模式` 两种，一般来说不需要走全局模式，PAC 模式则需要配合过滤规则使用，以判断数据是否需要代理，如果使用 PAC 模式，在 Chrome 下可配合 SwitchyOmega 扩展来使用（原先叫 SwitchSharp 现在改名了。

### Chrome

[SwitchyOmega](https://github.com/FelisCatus/SwitchyOmega/releases) 托管在 Github 上，可直接下载 `.crx` 文件后拖入 `Chrome > 设置 > 扩展程序` 列表进行安装安装。

安装后在 SwitchyOmega 的选项内的 `代理` 标签下修改

    网址协议：默认
    代理协议：设置为 SOCKS5
    代理服务器：127.0.0.1
    代理端口：填入先前设置的 "local_port"，这里使用的是 1080

然后应用选项，完成设置。使用 SwitchyOmega 选择『代理』模式即可正常上网。  

### 移动客户端

安卓客户端在 Google Play Store 可以找到 —— [Shadowsocks](https://play.google.com/store/apps/details?id=com.github.shadowsocks)，开发者 Max Lv，安装后输入相应数据就可以配置完成。

iOS 端还没测试过。

## 其它

-   费用：DigitalOcean 最基础的配置是 5USD/月、0.007USD/，两种算法差不多价格，如果长时间不需要正常上网，可以登录服务器停止服务。[DigitalOcean](https://www.digitalocean.com/?refcode=08c5123baa50) 使用我的推荐链接注册会得到 $10 额度，大概是两个月的使用时间。
-   协议：如果不使用 Shadowsocks，也可以搭建使用其它协议的 VPN，知乎这个[如何自行架设VPN服务](http://www.zhihu.com/question/20113381)的问题下有挺多资料，[这篇回答](http://www.zhihu.com/question/20113381/answer/29554581)图片较多，可以参考。
-   不推荐购买售卖的 Shadowsocks 服务。

## 参考

搭建和写作过程中所参考的资料

-   [从零开始VPS](http://jecvay.com/2014/12/learning-vps-1/)
-   [shadowsocks+digitalOcean科学上网体验教程](http://www.csdn123.com/html/topnews201408/36/3736.htm)
-   [DigitalOcean VPS 翻墙体验](http://wuchong.me/blog/2014/10/17/digitalocean-vps-experience/)
-   [SwitchyOmega](https://github.com/FelisCatus/SwitchyOmega)
-   [Shadowsocks](http://shadowsocks.org/)

p.s. 准备更新这篇文章时发现 Instagram 解封了，禁掉一些又解开一些，水冒泡而不沸腾 :)