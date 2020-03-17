---
title: Hadoop 系统安装之 Linux 系统安装
permalink: Hadoop系统安装之Linux系统安装/
date: 2018-10-09
categories:
 - [Hadoop]
 - [Linux]
tags: 
 - Hadoop
 - Linux
keywords: [hadoop, linux安装]
---

本篇主要讲述安装 Hadoop 系统的前提：Linux 系统的安装，因为大多数人日常都是使用 Windows 系统，但是 Hadoop 只能在 Linux 下安装，今天就来说说几种安装 Linux 的方法，仅供参考。

<!-- more -->

### Windows Subsystem for Linux(Windows Linux 子系统）

**安装的 Linux 默认没有图形界面，如果需要图形界面的话可以去搜索教程，各种各样的都有，没几步，很简单**

首先要开启 WSL 框架组件

![开启 WSL](https://image-1253491707.file.myqcloud.com/20181009193626.png/webp)

重启后打开 Microsoft store 搜索 Linux ，会发现有许多的 Linux 系统可供选择，在这里我选择 Ubuntu 安装

![linux](https://image-1253491707.file.myqcloud.com/20181009194708.png/webp)

安装之后就可以启动 Ubuntu 了，打开之后会进行初始化，等一会儿就好了。初始化之后会提示新建账户，用户名不与 Windows 用户名互通。密码输入是非明文的，不会显示*号，不要以为是键盘坏了。..（当然此时是可以直接关掉重开的，重开时会默认以 root 账户登录）

新建个普通用户的话每次敏感操作都需要带个 `sudo` 当然可以在命令行输入 `sudo -i` 提权。

子系统搞好的第一件事就是更换 apt 源，因为吗，默认的源是国外的，访问肯定没有国内的源快。

{% tabs apt源, 1 %}
<!-- tab 阿里源 -->
{% codeblock /etc/apt/sources.list lang:bash %}
deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
{% endcodeblock %}
<!-- endtab -->

<!-- tab 科大源 -->
{% codeblock /etc/apt/sources.list lang:bash %}
deb https://mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
{% endcodeblock %}
<!-- endtab -->

<!-- tab 网易源 -->
{% codeblock /etc/apt/sources.list lang:bash %}
deb http://mirrors.163.com/ubuntu/ wily main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ wily-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ wily-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ wily-proposed main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ wily-backports main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ wily main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ wily-security main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ wily-updates main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ wily-proposed main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ wily-backports main restricted universe multiverse
{% endcodeblock %}
<!-- endtab -->

{% endtabs %}

apt 源的路径是 `/etc/apt/sources.list` ，先打开此文件夹

```shell
nano /etc/apt/sources.list
```

然后将上面的一个源复制粘贴到文件的最前面（右击鼠标粘贴）

保存后在命令行输入 `apt update` 更新配置

如果你想愉快地使用 WSL 的话，建议替换一下 Windows 自带的 cmd，不仅不好用还丑，cmder、gitbash 等都还不错，想替换的可以搜索`Windows cmd 替换工具`自行选择，这里不多做赘述。当然选择 SSH 登录也是一种不错的方案。这种方式等下再讲。

### 虚拟机

虚拟机方案有两种，下载软件或者是开启 Windows 自带的 hyper-V 虚拟机组件，hyper-V 在 Windows 功能中启用就好，虚拟机软件的话我这里放两个出来，其他的一些就不说了，软件都大同小异，重点还是学习 Linux。

[VMware Workstation Pro](http://www.dayanzai.me/vmware-workstation.html) ：VMware Workstation 是虚拟机中的佼佼者，大名鼎鼎

[VMware Player 15.0.0](http://www.dayanzai.me/vmware-player.html) ：VMware Player 是 VMware 推出的免费精简版本，简单但不简陋，也是一个不错的选择

### Windows、Linux 双系统

此方案网上也有很多教程，但是有些教程说的不清楚，如果想尝试建议做好崩的心理准备，我自己就是装的双系统，也是崩了几次才装好的。因为不同的 Windows 引导方式，单双硬盘安装时都有点区别，不对不仅是 Linux 装不好就连 Windows 也会崩掉，建议做好备份多查阅资料再去尝试。
