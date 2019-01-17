---
layout:     post
title:     Python环境变量PYTHONPATH设置导入第三方模块
subtitle:  修改环境变量路径以便导入模块
date:       2018-03-06
author:     转载
header-img: img/post-bg-python.jpeg
catalog: true
tags:
    - python
---

# 1.暂时设置模块的搜索路径——修改sys.path
  我们在导入模块的时候，python会在指定的路径下搜索相对应的.py文件，搜索路径存放在sys模块的sys.path变量中，如下图

![img](https://s2.ax1x.com/2019/01/17/kpm4x0.png)

这个path变量是一个列表，因此我们可以通过append函数在其后添加搜索路径，如果我们要导入的第三方模块的路径是'D:\Users',那在python解释器中添加sys.path.append('D:\\Users'),注意为什么我这里是'\\',因为我这里的sys.path变量里的格式是'\\',根据自己sys.path变量里的格式来，设置好以后可以查看一下，如下图：

![img](https://s2.ax1x.com/2019/01/17/kpmfGn.png)

添加好路径以后就可以导入你要导入的模块了，但是这种方法只是暂时的，下次你再进入交互模式的时候又得重新设置。是不是很麻烦？

# 2.永久设置模块的搜索路径——设置PYTHONPATH环境变量
  网上的大佬们写的设置PYTHONPATH环境变量，我一直没读懂到底是怎么设置的，直到刚刚偶然的机会，看到一个外国友人的回答，原来是这样啊！是我自己想复杂了，其实就是添加一个名为PYTHONPATH的系统环境变量。

  右击【我的电脑】-【属性】-【高级系统设置】-【环境变量】-【新建】，变量名写PYTHONPATH，变量值就是你要导入模块的路径了，以后还要导入其他模块，就继续在后面添加路径，至此，已经设置好了。

![img](https://s2.ax1x.com/2019/01/17/kpmh2q.png)

![img](https://s2.ax1x.com/2019/01/17/kpmWPs.png)
