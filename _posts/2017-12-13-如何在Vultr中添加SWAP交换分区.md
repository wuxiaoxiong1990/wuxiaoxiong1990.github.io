---
layout:     post
title:      如何在Vultr中添加SWAP交换分区
subtitle:   提升小内存VPS性能
date:       2017-12-13
author:     转载
header-img: img/post-bg-ioses.jpg
catalog: true
tags:
    - swap
---

Vultr 和DigitalOcean 的VPS 主机在开新方案后，预设置是不会有swap 交换分区，不像Linode 在管理后台Rebuild 时，就可以设置swap 分区大小。

swap 要设置多大，可以依以你的VPS RAM 来判断。

比如：1GB RAM 那你的swap 交换分区就可以设置1024MB，Vultr 768RM 方案，swap 可以设置512MB，一般最大可以设置到8GM。

不过我也碰过老外工程师设置过独服swap 交换分区32GB，可能认为我的HD 不用钱吧 !

如何手动添加swap 呢? ( CentOS / Ubuntu )

#### 检查Swap 空间

在设置swap 文件之前，有必要先检查一下系统里有没有既存的swap 文件。

```
swapon -s
```

如果返回的信息概要是空的，则表示swap 文件不存在。

#### 检查文件系统

在设置swap 文件之前，同样有必要检查一下文件系统，看看是否有足够的硬碟空间来设置swap。

```
df -hal
```

检查返回的信息，还剩余足够的硬碟空间即可。

#### 创建swap

下面使用dd 命令来创建swap 文件。

```
dd if=/dev/zero of=/swapfile bs=1024 count=524288 
```
回覆讯息
```
524288+0 records in
524288+0 records out
536870912 bytes (537 MB, 512 MiB) copied, 2.2845 s, 235 MB/s
```
#### 格式化并启动swap

上面已经创建好swap 文件，还需要格式化后才能使用。
```
mkswap /swapfile
```
回覆讯息

```
Setting up swapspace version 1, size = 512 MiB (536866816 bytes)
no label, UUID=b247ba10-f7cf-47b7-aa65-f69b2cfcc8fd
```
#### 启动swap
```
swapon /swapfile
```
回覆讯息
```
swapon: /swapfile: insecure permissions 0644, 0600 suggested.
```
以上步骤做完，再一次运行命令：
```
swapon -s
```
回覆讯息
```
Filename                Type        Size    Used    Priority
/swapfile               file        524284    0     -1 
```
如果要VPS主机重启的时候自动挂载swap ，那么还需要修改fstab配置。
用vim打开/ etc / fstab文件，在其最后添加如下一行：
```
vi /etc/fstab
```
```
/swapfile          swap            swap    defaults        0 0
```
最后，赋予swap 文件适当的权限：
```
chown root:root /swapfile
chmod 0600 /swapfile
```
同时，我们还可以修改Linux swap空间的swappiness ，降低对硬碟的缓存。
Linux会使用硬碟的一部分做为swap分区，用来进行进程调度。

进程是正在运行的程序，把当前不用的进程调成「等待( standby )」，甚至「睡眠( sleep )」。

一旦要用，再调成「活动( active )」，睡眠的进程就会在swap 分区，把内存空出来让给「活动」的进程。

如果记忆体够大，应当告诉Linux 不必太多的使用swap 分区，可以通过修改swappiness 的参数来设置。

swappiness = 0 的时候表示最大限度使用物理内存，然后才是swap 空间。

swappiness = 100 的时候表示积极的使用swap 分区，并且把内存上的数据及时的搬运到swap 空间里面。

在CentOS 中，swappiness 的默认值是60。

通过以下命令可以看到：
```
cat /proc/sys/vm/swappiness
```
返回值60

我们可以调整swappiness 的值到一个合适的参数，从而达到最优化使用swap 的目的。

这里我们将其设为10。使用sysctl 命令：
```
sysctl vm.swappiness=10 
```
但是这只是临时性的修改，在你重启系统后会恢复默认的60，要永久设置，还需要在vim 中修改sysctl.conf：
```
vi /etc/sysctl.conf
```
在这个文档的最后加上这样一行：

```
# Search for the vm.swappiness setting.  Uncomment and change it as necessary.
vm.swappiness=10
```
输入:qw，保存退出vim 。

这样一来，swap 分区重启后都会生效了。

补充swap file 计算方法：

建立512MB 的swap，一次读写1024bytes

bs=1024 count=524288 # 1024 * 512M = 524288 block size