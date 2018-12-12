---
layout:     post
title:      Nextcloud离线下载搭建方法
subtitle:   整合Aria2和AriaNg、Aria2 WebUI实现离线下载
date:       2017-9-26
author:     Qi
header-img: img/post-bg-keybord.jpg
catalog: true
tags: 
    - nextcloud
    - 离线下载
---

转自[挖站否](https://wzfou.com/)

Nextcloud是一个非常优秀的私有云存储服务，利用官网提供的Snap安装Nextcloud方法，几乎可以几分钟内就可以搭建好Nextcloud云存储平台。Nextcloud提供了丰富的应用接口，不仅仅可以将Nextcloud当成是网盘使用，还可以在线查看文档、图片和播放视频音乐等。

因为[Nextcloud](https://wzfou.com/tag/nextcloud/)的强大功能，不少的朋友可能想到能不能利用Nextcloud来搭建一个离线下载平台。其实，作为Nextcloud的前身，OwnCloud就已经提供了离线下载的插件，只不过安装与配置起来比较复杂一些。到目前为止，Nextcloud暂未提供可供使用的离线下载工具。

不过，我们完全可以利用Aria2配合NextCloud实现离线下载存储与在线观看播放的效果。Aria2是一个非常优秀的支持多种协议的轻量级命令行下载工具，优点是：多线程连线充分利用带宽；运行时不会占用过多资源，通常在 4MB~9MB；全功能 BitTorrent 客户端； 支持 RPC 界面远程控制。

[AriaNg](https://wzfou.com/tag/ariang/)就是一个是运行在服务端的Aria2前端管理工具，它可以不用Aria2命令就可以在网页上添加下载任务。当然，本篇文章还为大家介绍一种在本地安装[Aria2 WebUI](https://wzfou.com/tag/aria2-webui/)实现本地操控Aria2离线下载的方法。总之，配合好离线下载，Nextcloud又可以变身为办公与娱乐平台了。

[![Nextcloud离线下载搭建方法-整合Aria2和AriaNg\Aria2 WebUI实现离线下载](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_00.jpg)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_00.jpg)

更多的实用建站工具与程序，你可以看看：

1. [HashOver免费开源PHP评论系统安装使用-自建评论系统替代第三方](https://wzfou.com/hashover/)
2. [Lsyncd搭建同步镜像-用Lsyncd实现本地和远程服务器之间实时同步](https://wzfou.com/lsyncd/)
3. [接入CN2线路VPS主机商和机房汇总-鉴别真假CN2线路主机参考手册](https://wzfou.com/cn2-vps-list/)

**PS：2017年10月14日更新，**有兴趣树莓派Raspberry Pi与Nextcloud整合的朋友可以看看：[树莓派Raspberry Pi安装NextCloud教程-自建家庭私有云局域网共享](https://wzfou.com/raspberry-nextcloud/)。

## 一、Nextcloud安装使用

Nextcloud安装与使用我在下面两篇文章中已经详细地进过了，喜欢折腾的朋友，可以自己搭建LNMP和LAMP手动安装Nextcloud，对于只想马上上手Nextcloud的朋友，建议使用一键安装方法。

1. [Nextcloud个人云存储绝佳选择：一键自动安装方法和云盘使用体验](https://wzfou.com/nextcloud/)
2. [手动安装NextCloud教程-免费开源的私有云存储网盘可播放图片音乐](https://wzfou.com/nextcloud-install/)

## 二、在VPS上安装Aria2

执行命令安装：

```
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/aria2.sh && chmod +x aria2.sh && bash aria2.sh
#备用地址：https://www.ucblog.net/wzfou/aria2.sh
```

运行脚本后，你可以安装、升级Aria2。

[![Nextcloud离线安装脚本](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_01.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_01.gif)

**修改Aria2下载存储路径**。打开：vi /root/.aria2/aria2.conf，找到：dir=XXX，修改为Nextcloud的存储路径。

[![Nextcloud离线修改存储路径](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_03.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_03.gif)

为了保证安全性，请将此路径以外挂存储的方式，在Nextcloud的管理面板中进行挂载。当然不想以挂载的方式，那么请在上面将路径设置为Nextcloud默认的存储路径。

[![Nextcloud离线调整路径](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_02.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_02.gif)

**修改RPC令牌(rpc-secret)。**RPC令牌就相当于 [Aria2](https://wzfou.com/tag/aria2/)(后端/服务端)远程API连接的授权密码，如果你想让任何人都使用的话，你可以将RPC令牌留空，否则请设置为你自己的密码。

[![Nextcloud离线设置令牌](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_04.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_04.gif)

## 三、AriaNg下载与使用

AriaNg项目：

1. 项目：https://github.com/mayswind/AriaNg
2. 下载：https://github.com/mayswind/AriaNg/releases/latest

AriaNg是一个前端(HTML+JS静态)控制面板，不需要和 Aria2(后端/服务端)放在一个服务器或者设备中，你可以直接下载到你的本地电脑上解压打开index.html，或者放在服务器访问，服务器只要有Nginx或者Apache就可以了。

点击打开AriaNg 设置 填入RPC别名、地址、协议、请求方法和密钥。RPC地址填写IP或者域名，端口默认的是6800，密钥的话就是你刚刚在配置文件中修改过的。（点击放大）

[![Nextcloud离线设置密钥](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_05.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_05.gif)

设置完成后，点击Aria2 状态你可以看到Aria2已经连接成功了。没有连接成功的话，检查一下VPS的防火墙有没有开放两个端口，一个是RPC监听端口` 6800(默认)`，一个是BT监听端口` 51413(默认)`。当然修改了配置文件后记得重启VPS。

[![Nextcloud离线连接成功](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_05_1.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_05_1.gif)

打开AriaNg面板，你就可以添加http\BT\磁力链接开始下载了。

[![Nextcloud离线添加下载任务](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_07.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_07.gif)

由于我们用的是VPS主机下载资源，所以速度基本上可以飞起来了。

[![Nextcloud离线速度非常快](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_08.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_08.gif)

## 四、Aria2 WebUI安装使用

Aria2 WebUI

1. 项目：https://github.com/ziahamza/webui-aria2

你可以将[Aria2 WebUI](https://wzfou.com/tag/aria2-webui/)放在本地或者是放在服务器上，使用方法和上面的AriaNg差不多。运行Aria2 WebUI，然后在设置中选择连接设置。主机就填写你的VPS主机IP地址，端口默认是6800，访问密码就是你修改Aria2配置文件的RPC令牌。

[![Nextcloud离线本地设置](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_10.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_10.gif)

设置成功后，你就可以在Aria2 WebUI中看到Aria2下载任务了，同时你也可以添加链接、种子、磁力链接开始下载了。

[![Nextcloud离线开始下载了](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_11.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_11.gif)

## 五、Nextcloud离线下载使用

按照上面的方法我们已经实现了离线下载，打开Nextcloud网盘，你就可以看到Aria2 下载的文件了。

[![Nextcloud离线看到下载的文件](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_18.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_18.gif)

Nextcloud自带了同步客户端，你可以利用客户端将文件下载到本地，当然Nextcloud可以直接在线播放音乐、视频等，流量足够的话直接自己在线观看即可。

[![Nextcloud离线在线观看](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_19.jpg)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_19.jpg)

## 六、本地+Aria2+百度网盘

**如果你没有VPS主机，也没有Linux，也没有关系，**这里提供一个本地运行Aria2的方法，支持Mac和Windows，同时还提供Chrome插件，帮助你直接获取百度网盘的文件下载地址，跳过百度网盘客户端，直接使用Aria2高速下载网盘文件。

1. 下载：https://www.ucblog.net/wzfou/Aria2.zip

下载安装包，解压后有三个文件夹，其中Plugin是Chrome插件，我用过之后获取百度网盘不一定有效，可以用本文介绍的安装油猴子的方法来解决。**Mac OS安装Aria2GUI.dmg，位于网盘的Aria2 for Mac文件夹中。**

[![Nextcloud离线三个文件](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_08_1.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_08_1.gif)

**Windows用户的话，**进入Aria2 for Windows，将aria2.rar这个文件解压在D:\aria2这个文件夹里，即D:\aria2\。然后在D盘根目录建立一个Downloads的文件夹，这个文件夹就是你下载的文件存放的地方。

[![Nextcloud离线解压文件](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_09.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_09.gif)

进入D:\aria2\里面，双击HideRun.vbs这个文件，然后进入任务管理器可以看到aria2c.exe这个进程正在运行。找到 aria2控制界面.rar，将这个文件在任意位置解压缩，然后双击index.html这个文件，你的默认浏览器就会打开。

[![Nextcloud离线在本地运行](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_19.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_19.gif)

接下来你就会进入到Aria2 WebUI控制面板，添加下载地址，跟上面的操作是一样的。不过，为了可以下载百度网盘中的文件，你需要在Chrome上安装tampermonkey应用，然后到greasyfork.org下载安装脚本，只要跟百度有关的你都可以安装。

[![Nextcloud离线安装插件](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_13.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_13.gif)

现在用浏览器打开百度网盘，然后在下载页面就会出导出下载链接的按钮了。

[![Nextcloud离线导出下载地址](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_14.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_14.gif)

复制出下载链接地址，然后放在Aria2 WebUI和AriaNg中开始调用Aria2下载了。

[![Nextcloud离线下载百度网盘](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_15.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_15.gif)

如果速度太慢的话，你可以修改下载连接数。

[![Nextcloud离线修改连接数](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_16.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_16.gif)

你也可以修改下载的Agent，这样可以逃避百度的封锁或者躲开一些不让爬虫下载的页面。

[![Nextcloud离线修改浏览器标识](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_17.gif)](https://wzfou.cdn.bcebos.com/wp-content/uploads/2017/08/Aria2-nextcloud_17.gif)

## 七、总结

从上文应该能看出来，我们将[Nextcloud与Aria2](https://wzfou.com/tag/nextcloud-aria2/)整合，只是将各自的优势整合在一块了。Aria2负载下载文件，Nextcloud可以管理查看文件。而Aria2的控制面板又可以完全脱离Web服务器，直接在本地运行也可以。

你甚至可以将它放在Nextcloud同步中，这样在任意地点都可以打开Aria2和AriaNg\Aria2 WebUI来查看和添加下载任务了。当然，在使用Aria2和AriaNg\Aria2 WebUI实现离线下载前，注意国外的VPS主机对版本文件比较严格。

文章出自：[挖站否](https://wzfou.com/) <https://wzfou.com/nextcloud-aria2/>，部分内容参考自：[Justin Smith](https://medium.com/@Justin___Smith/aria2%E9%85%8D%E7%BD%AE%E6%95%99%E7%A8%8B-mac%E5%92%8Cwindows-b31d0f64bd4e) 、[doub.io](https://doub.io/wlzy-30/)  版权所有。本站文章除注明出处外，皆为作者原创文章，可自由引用，但请注明来源。