---
title: clash-for-windows-guide
tags:
  - Over The Wall
---

Clash 是使用 GO 编写的多平台代理工具，支持规则分流，最近更是支持了 ssr，下面就来讲一下 clash for windows 的使用教程。

<!-- more -->

## 下载安装

首先去 [GitHub release](https://github.com/Fndroid/clash_for_windows_pkg/releases) 下载最新版本的 CFW，下载完成后安装并打开 CFW。

![CFW 主界面](https://image-1253491707.file.myqcloud.com//20200731222100.png)

## 基础配置

然后点击侧栏 `Profiles` 添加你自己的配置文件或者订阅，这里只讲订阅，将链接填入输入框然后点击 `Download` 按钮下载配置文件。

下载完成后点击新添加的配置文件以切换到此配置（点击按钮以外的地方）。

然后点击 `info` 按钮修改配置文件的信息，将 `Update Interval` 设置为 12（每 12 小时自动更新配置文件）后保存。

![change information](https://image-1253491707.file.myqcloud.com//20200731223334.png)
![Update Interval](https://image-1253491707.file.myqcloud.com//20200731224010.png)

此时应该已经可以国际互连了（需要在想代理的软件中使用代理服务器），不过想要用的舒心的话，还需要做一下设置：

1. 开机自启：`General` 界面打开 `Start with Windows`
2. 系统代理：`General` 界面打开 `System Proxy`

此时大多数软件都应该可以访问国际互联网了，但是如果你想 uwp 应用也使用代理的话就必须去 `General` 界面配置 `UWP Loopback`，不过我更推荐使用 Tap 模式。

Tap 模式不需要配置 `UWP Loopback`，也不需要打开 `System Proxy`，已经配置或打开的记得关闭。

首先点击 `General` 界面 `Tap Device` 的 `Manage` 安装 TAP 网卡

然后 点击 `Settings` 界面 `Profile Mixin` 的 YAML Edit 按钮，并将一下内容复制进去。

```yaml
mixin: # object
  dns:
    enable: true
    listen: 0.0.0.0:53
    enhanced-mode: fake-ip
    nameserver:
      - 119.29.29.29
      - 223.5.5.5
    fake-ip-filter: # fake ip white domain list
      - '*.lan'
      - localhost.ptlogin2.qq.com
      - dns.msftncsi.com
      - www.msftncsi.com
      - www.msftconnecttest.com
```

![Profile Mixin](https://image-1253491707.file.myqcloud.com//20200731225948.png)

然后将 `General` 的 `Mixin` 功能打开即可。此时不管是应用，UWP，命令行终端还是 WSL 都可以走代理啦。推荐使用这种方式。

## 配置解析

新手可能会对配置文件一头雾水，这里就重点讲一下最重要的策略组与分流。

一般的配置文件都有两种策略组:

1. 使用地区进行分类，将同一地区的节点分到同一策略组中
   ![地区策略组](https://image-1253491707.file.myqcloud.com//20200731231503.png)

2. 根据不同网站的共同特点进行分类，将节点或者策略组分到一个策略组中
   ![特性策略组](https://image-1253491707.file.myqcloud.com//20200731231808.png)
   就像我的图中 Proxy 就是嵌套策略组，域名或者 ip 满足 Proxy 规则时就会通过 Proxy 策略组里我选定的地区策略组里的某一台服务器。

一般的配置文件都会有这么几个策略组：
others：未命中任何规则时的默认策略
Adblock：广告屏蔽
DIRECT：直接连接，不通过代理
REJECT：丢弃，屏蔽这个域名

了解了这些之后使用应该没有多大的问题了，想要了解更多内容的话可以查看 [CFW 手册](https://docs.cfw.lbyczf.com/)
