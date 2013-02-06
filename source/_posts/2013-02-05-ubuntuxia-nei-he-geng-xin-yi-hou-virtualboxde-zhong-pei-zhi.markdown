---
layout: post
title: "ubuntu下内核更新以后virtualbox的重配置"
date: 2013-02-05 23:47
comments: true
categories: linux
---
随着 Ubuntu 系统内核的更新，VirtualBox 原有的内核模块已经不再适用。于是，VirtualBox 将无法正常使用。需要重新安装 VirtualBox 吗？当然不必。我们只需重新稍加配置即可。 

首先获取与当前内核版本相一致的头文件： 

```
$ sudo apt-get install linux-headers-`uname -r` 
```

接着，我们来重新编译 VirtualBox 内核模块，这可以使用下面的指令完成： 

```
$ sudo /etc/init.d/vboxdrv setup 
```

一旦编译完成，程序将会自动启动 vboxdrv 内核模块。此时，再用 VirtualBox 也就没有什么问题了。如果以后遇到内核再次重新的情况，则如法炮制即可解决。