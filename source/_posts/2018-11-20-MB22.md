---
title: Ubuntu & VSCode Powerline 使用记录
categories: 反正好用就对了
tags:
 - Powerline
 - linux
 - 反正好用就对了
keywords: [powerline, vscode 插件, vscode, 终端美化]
---

前几天在网上偶然看到有人的 vscode 终端是彩色的，感觉比自带的好多了，本来以为是扩展，搜了一下又没找到，后来才知道这是一种字体。再后来知道了 powerline 这个东西，powerline 是针对 vim、bash 等的一个状态栏插件。他给程序提供了状态栏并使程序更加好看。它用 Python 写成。今天就来讲讲如何在 Ubuntu 上安装使用它。

<!-- more -->

## 安装前提

1. 电脑环境为 ubuntu 18.04，其他的系统自行替换相应的包管理命令，其他的应该大同小异。
2. 默认已安装 Python 以上 及 git

[点击这里](#%E5%9C%A8-VS-code-%E4%B8%AD%E4%BD%BF%E7%94%A8-powerline) 直接跳转到 VS Code 上使用 powerline

## 安装 Powerline

Ubuntu apt 命令可直接安装 powerline，只需要 `sudo apt install powerline` 就好。接下来讲讲手动安装。

首先安装 pip，

``` bash
sudo apt install python-pip
```

然后安装 powerline

``` bash
pip install --user powerline-status
```

## 启用 powerline

找出 powerline 安装位置以便配置程序。

``` bash
pip show powerline-status
Name: powerline-status
Version: 2.7.dev9999-git.b-5d82d544f3366a214732aeb8f781d50ced38ceef-
Summary: The ultimate statusline/prompt utility.
Home-page: https://github.com/powerline/powerline
Author: Kim Silkebaekken
Author-email: kim.silkebaekken+vim@gmail.com
License: MIT
Location: /usr/local/lib/python3.6/dist-packages
Requires:
```

###　在 shell 中启用 powerline
添加下面的行到 .bashrc 中，它会默认在基础 shell 中启用 powerline。

``` vim
if [-f `which powerline-daemon`]; then
  powerline-daemon -q
  POWERLINE_BASH_CONTINUATION=1
  POWERLINE_BASH_SELECT=1
  . /usr/local/lib/python3.6/dist-packages/powerline/bindings/bash/powerline.sh
fi
```

重新加载 .bashrc 文件使得 powerline 在当前窗口中立即生效。

``` bash
source ~/.bashrc
```

### 在 vim 中启用 powerline

``` vim
nano ~/.vimrc
set  rtp+=/usr/local/lib/python3.6/dist-packages/powerline/bindings/vim/
set laststatus=2
set t_Co=256
```

默认可能这个文件，创建就是了。

## 字体安装

### Fontconfig 方式

此方法仅适用于 linux，如果终端支持的话使用这种方法就不需要下载修补字体，对大多数字体都是有效的。

下载最新的符号字体和 fontconfig 文件

``` bash
wget https://github.com/powerline/powerline/raw/develop/font/PowerlineSymbols.otf
wget https://github.com/powerline/powerline/raw/develop/font/10-powerline-symbols.conf
```

列出有效的 X 字体路径, 查看下图可知应该将字体移到 `/usr/share/fonts/X11` 目录下

``` bash
xset q
```

![xset q](https://blog-1253491707.piccd.myqcloud.com/imgs/20181120130005.png/style)

将符号字体移动到有效的 X 字体路径。

``` bash
sudo mkdir /usr/share/fonts/X11/powerline
sudo mv PowerlineSymbols.otf /usr/share/fonts/powerline
```

更新字体缓存

``` bash
sudo fc-cache -fv
```

安装 fontconfig 文件。

``` bash
sudo mv 10-powerline-symbols.conf /usr/share/fontconfig/conf.avail
```

### 安装修补字体

安装修补字体是首选的方法，最快也最简单。但只有少数修补了的字体才有效。

修补字体一般都带有 powerline 字样，[powerline 项目下](https://github.com/powerline/fonts) 有一些字体。或者搜索关键字寻找其他的字体。

下载字体后进行以下操作即可

将字体移动到有效的 X 字体路径。没有 powerline 路径可自行创建或使用父目录。

``` bash
sudo mv somefont for powerline.otf/ttf /usr/share/fonts/X11/powerline
```

更新字体缓存

``` bash
sudo fc-cache -fv
```

## 在 VS code 中使用 powerline

修改 VSC 的终端只需要将 powerline 修补字体安装或移动到字体文件夹。修补字体安装见上文。

然后在 VSC 中的 user settings 指定终端字体即可

``` json
// 控制终端的字体系列，默认为 `editor.fontFamily` 的值。
"terminal.integrated.fontFamily": "powerline consolas",
```