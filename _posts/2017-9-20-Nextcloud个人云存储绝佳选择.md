---
layout:     post
title:      Nextcloud个人云存储绝佳选择
subtitle:   一键自动安装方法和云盘使用体验
date:       2017-9-20
author:     Qi
header-img: img/post-bg-keybord.jpg
catalog: true
tags:
    - nextcloud
---



文章出自：[挖站否](https://wzfou.com/) <https://wzfou.com/nextcloud/>

搭建个人云存储一般会想到ownCloud，堪称是自建云存储服务的经典。而[Nextcloud](https://wzfou.com/tag/nextcloud/)是ownCloud原开发团队打造的号称是“下一代”存储。初一看觉得“口气”不小，刚推出来就重新“定义”了Cloud，真正试用过后就由衷地赞同这个Nextcloud：它是个人云存储服务的绝佳选择。

与ownCloud相比，Nextcloud的功能丝毫没有减弱，甚至由于可以安装云存储服务应用，自制性更强，也更符合用户的需求。Nextcloud官网的帮助文档写得相当地详细，几乎任何关于Nextcloud的问题都可以找到答案，这说明Nextcloud开发团队确实比ownCloud更加优秀。

一开始以为Nextcloud只是一个网盘[云存储](https://wzfou.com/tag/siyou-yuncunchu/)，后来看到Nextcloud内置了Office文档、图片相册、日历联系人、两步验证、文件管理、RSS阅读等丰富的应用，我发现Nextcloud已经仅仅可以用作个人或者团队存储与共享，还可以打造成为一个个人办公平台，几乎相当于一个个人的Dropbox了。

Nextcloud运行环境与平常我们常用的程序差不多，LAMP是官方首选，不过LNMP也照样可以运行，只不过需要自己写URL重写规则。当然，官方还提供了SNAP一键安装包，一分钟内就可以在VPS上部署好Nextcloud，非常地方便。本篇文章就来分享SNAP安装Nextcloud的方法。

[![Nextcloud个人云存储绝佳选择:一键安装自带免费客户端内置文档\相册\日历丰富应用](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_00.jpg)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_00.jpg)

更多的关于邮件分发、CDN加速和[VPS同步](https://wzfou.com/tag/vps-tongbu/)等工具，你还可以看看：

1. [利用MailChimp建立RSS邮件订阅平台-每月免费12000封邮件可加2000用户](https://wzfou.com/mailchimp/)
2. [用Fikker自建CDN-支持Https,页面缓存,实时监控,流量统计,防CC攻击](https://wzfou.com/fikker/)
3. [Lsyncd搭建同步镜像-用Lsyncd实现本地和远程服务器之间实时同步](https://wzfou.com/lsyncd/)

**PS：2017年9月27日更新，**想要利用Nextcloud实现离线下载可以看这里：[Nextcloud离线下载搭建方法-整合Aria2和AriaNg、Aria2 WebUI实现离线下载](https://wzfou.com/nextcloud-aria2/)。

## 一、Nextcloud一键安装

Nextcloud官网：

1. https://nextcloud.com/
2. nextcloud snap：https://github.com/nextcloud/nextcloud-snap

[nextcloud snap](https://wzfou.com/tag/nextcloud-snap/)目前包含以下组件（会自动更新升级，请及时关注）：

> Nextcloud 11.0.3
>
> Apache 2.4
>
> PHP 7
>
> MySQL 5.7
>
> Redis 3.2
>
> mDNS for network discovery

**安装前修改好hostname。**在终端窗口中输入命令：hostname或uname –n，均可以查看到当前主机的主机名，修改参考如下（Ubuntu修改可参考我之前的一篇文章：[ISPConfig 3.1 安装方法](https://wzfou.com/ispconfig-3-1/#ISPConfig_31)）：

```
vim /etc/hosts
150.95.150.57 pan.wzfou.net pan  

vim /etc/hostname
pan.wzfou.net
hostname -F /etc/hostname #重启
hostname #再次查看
```

**一键安装方法：**

```
sudo apt-get update
sudo apt install snapd
sudo snap install nextcloud
```

如下图表示安装成功了。

[![Nextcloud一键安装成功](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_01.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_01.gif)

接着，打开你的域名或者IP地址，然后会让你设置好管理员账号与密码，确定，完成安装。

[![Nextcloud打开域名](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_02.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_02.gif)

可能有的人不想使用Snap安装方法，可以看看手动在VPS上部署LNMP/LAMP安装Nextcloud方法：[手动安装NextCloud教程](https://wzfou.com/nextcloud-install/)。

**选择Snap还是VPS手动？**不用纠结，以下官方的回答：

> **snap优点**：The snap is nice for getting up and running quickly with minimal space, and will work great if you’re happy not messing with it. Since it’s a snap you also get the benefit of automatic updates and the ability to rollback without worrying about needing to take a snapshot, etc.
>
> \#翻译：snap安装快捷，傻瓜式一键安装，几分钟内搞定。同时，snap支持自动升级、回滚等，你无需使用复杂的命令工具。
>
> **snap缺点：**However, the snap is very opinionated. Don’t want to use Apache? Sorry, the snap uses it. Don’t want to use MySQL? Sorry, MariaDB does not run on ARM. Something other than PHP 7.0.15? We picked the version we feel gives the best results. In other words, it’s not very tinker-friendly. We don’t do this to be mean, **we do this so that we can reliably update it without your needing to worry about it**.
>
> \#翻译：snap不能自定义，只能使用snap既定的MysqL、apache、PHP等。不过，这样的好处就是经过官方测试过的运行稳定且有利于后期自动升级。
>
> **VPS手动安装优缺点：**The VM is much more flexible. It’s a full version of Ubuntu server edition, allowing you to tweak whatever you need and it comes with many apps which are not that easy to configure for inexperienced administrators. This of course makes it larger. You’ll also need to make sure you maintain it and keep the OS up-to-date. Since it’s virtualized you can assign disk, CPU, memory, and network quotas to it (you’d need to install the snap in an lxc container or a VM to get the same abilities).
>
> \#翻译：VPS手动则比较灵活，你可以自已配置磁盘、CPU、内存和网络，但是同时你需要懂得如何维护好VPS操作系统。

## 二、Nextcloud管理使用

以下就是Nextcloud的管理中心面板，是不是与我们用过的Dropbox有点类似-简洁。左边就是分享的链接、收藏、WebDav地址，中间就是我们上传的图片、文档、程序等了，点击可以查看详情。右边有管理、个人、用户等。（点击放大）

[![Nextcloud用户中心](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_04.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_04.gif)

Nextcloud上传的视频支持在线播放。

[![Nextcloud视频播放](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_05.jpg)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_05.jpg)

Nextcloud上传的图片可以像幻灯片一样浏览。

[![Nextcloud浏览图片](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_06.jpg)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_06.jpg)

Nextcloud支持给分享的文档、图片等设置有效期、密码保护等，有点类似于百度网盘了。

[![Nextcloud分享文档](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_07.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_07.gif)

别人打开你的共享链接后就可以预览到图片或者视频了，也可以直接点击下载了。

[![Nextcloud下载文件](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_08.jpg)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_08.jpg)

在Nextcloud的个人中心页面，可以修改个人信息、应用密码、同步客户端等。

[![Nextcloud个人中心](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_09.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_09.gif)

Nextcloud在服务器管理页面，则可以查看CPU、内存等使用情况、切换Nextcloud主题、是否对存储在Nextcloud的文件进行加密、激活插件等。

[![Nextcloud服务器设置](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_09_1.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_09_1.gif)

## 三、Nextcloud同步客户端

Nextcloud提供了免费的同步客户端供大家下载使用，支持PC和手机。下载地址：https://nextcloud.com/install/#install-clients

[![Nextcloud客户端](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_03.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_03.gif)

在电脑上运行Nextcloud同步客户端，先填入你的Nextcloud地址。

[![Nextcloud运行客户端](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_10.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_10.gif)

然后就是输入Nextcloud的用户名以及客户端专用密码，这个专用密码需要到Nextcloud的个人中心页面生成。

[![Nextcloud填入专用密码](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_11.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_11.gif)

Nextcloud允许你选择同步某一个文件夹，还是同步整个Nextcloud账户。

[![Nextcloud同步文件夹设置](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_12.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_12.gif)

连接好了后，你就可以在本地看到Nextcloud同步过来的文件了，你在本地的操作都会影响到Nextcloud云端的文件存储，自动实现同步。

[![Nextcloud本地同步](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_13.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_13.gif)

以下是Nextcloud的手机同步客户端，功能差不多。

[![Nextcloud手机端](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_14.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_14.gif)

Nextcloud手机客户端支持自动上传文件，还有设置下载路径等等。

[![Nextcloud备份手机文件](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_15.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_15.gif)

## 四、Nextcloud添加应用

Nextcloud官方提供了非常多的应用：https://apps.nextcloud.com/，Office文档、图片相册、日历联系人、两步验证、文件管理、RSS阅读等丰富的应用。这些应用你可以手动下载安装，也可以直接在Nextcloud后台一键激活。

[![Nextcloud添加应用](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_22.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_22.gif)

**Office文档插件**：[Documents](https://apps.nextcloud.com/apps/documents)。有Collabora Online、Markdown Editor、Calendar、Onlyoffice、Documents等，其中Documents安装比较简单，直接启用即可。

[![Nextcloud查看Office文档](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_23.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_23.gif)

如果不支持打开Doc（X），你需要在你的Ubuntu安装以下包：

```
apt-get install libreoffice-writer
apt-get install libreoffice-common
apt-get install unoconv
```

Documents插件安装好了后就可以在线查看和编辑Office文档了。

[![Nextcloud在线编辑文档](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_24.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_24.gif)

**安全类插件**：[Two Factor TOTP Provider](https://apps.nextcloud.com/apps/twofactor_totp)。这个插件可以让你的Nextcloud账号支持开启登录两步验证。

[![Nextcloud两步验证](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_25.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_25.gif)

**RSS阅读器插件**：[News](https://apps.nextcloud.com/apps/news)。这个插件真的让我感觉眼前一亮，有了它我们可以将Nextcloud变身为一个RSS在线阅读器了。这个比之前我们[利用Huginn抓取任意网站RSS](https://wzfou.com/huginn-rss/)的方法可以简单了。（点击放大）

[![Nextcloud阅读器](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_26.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_26.gif)

## 五、Nextcloud高级设置

Nextcloud支持使用PHP发送邮件，但是自带的邮局发出去的邮件基本上是被各大邮箱判定为垃圾邮件，所以我们需要利用好Nextcloud提供的SMTP发信功能。

### 4.1  Nextcloud用SMTP发信

在Nextcloud的管理页面，找到“其他设置”，然后选择发信方式为SMTP，填写你的SMTP信息，这里我用的是腾讯企业邮箱的，你也可以使用Gmail、163等免费SMTP发信功能。

[![Nextcloud设置发信](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_20.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_20.gif)

填写完成后，点击测试看看是不是可以成功发出邮件。

[![Nextcloud成功发出邮件](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_21.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_21.gif)

### 4.2  Nextcloud安装SSL证书

如果你使用Snap安装的Nextcloud，那么添加SSL加密访问将是一件非常简单的事情。先确保你的域名已经成功解析到你的VPS主机上，然后执行命令：

```
sudo nextcloud.enable-https lets-encrypt #安装Let's Encrypt SSL
#如果你想使用自己的证书，请执行：
sudo nextcloud.enable-https self-signed
```

[![Nextcloud添加SSL证书](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_16.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_16.gif)

如果你是使用自已的证书，请在执行命令后找到SSL证书的路径，将自己的证书上传替换生成的自签名证书文件即可。

[![Nextcloud自己的证书](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_17.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_17.gif)

### 4.3  Nextcloud备份与恢复

**备份MysqL数据库**。使用Snap安装的Nextcloud，数据库文件在以下路径中，你直接将Nextcloud这个数据库全部备份即可。

[![Nextcloud备份数据库](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_18.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_18.gif)

**备份文件。**Nextcloud上传的文件存储在以下路径中，将里面的Data文件全部备份即可。

[![Nextcloud备份存储文件](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_19.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/nextcloud_19.gif)

**Nextcloud恢复。**重装安装了Nextcloud后，将上面的数据库文件与文件数据全部导入到新的服务器，保持原来的路径即可。对于大量的文件迁移，推荐使用远程搬家方法：[三个命令工具Rsync,SCP,Tar-快速解决Linux VPS远程网站搬家数据同步烦恼](https://wzfou.com/rsync-scp-tar/)。

## 六、总结

Nextcloud采用Snap的安装方法简单方便，适合不想折腾的朋友，并且官方打包的[Nextcloud Snap自动部署](https://wzfou.com/tag/nextcloud-install/)好了LAMP，如果你想迁移服务器，只需要将新的服务器按照同样的方法安装Nextcloud，然后导入之前的数据库与存储文件即可。

Nextcloud如果用来存储一些私人的照片或者文件的话，最让人担心的恐怕是安全问题了。目前来看，Nextcloud本身的安全措施已经做得非常到位，例如账号两步验证、程序与存储文件分开、数据加密等。可能唯一需要我们自己做的就是保证服务器不要出现漏洞。

文章出自：[挖站否](https://wzfou.com/) <https://wzfou.com/nextcloud/>，版权所有。本站文章除注明出处外，皆为作者原创文章，可自由引用，但请注明来源。