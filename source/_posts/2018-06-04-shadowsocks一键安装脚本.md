---
title: shadowsocks 一键安装脚本
permalink: shadowsocks一键安装脚本/
categories: Over The Wall
tags:
 - Over The Wall
---

GFW 经常会发力，有时候很多教程都被墙了，不方便查看，就想起来把自己使用的秋水逸冰大神的教程写下来以备不时之需。

<!-- more -->

## <font color="red">本脚本适用环境</font>

系统支持：CentOS 6+、Debian 7+、Ubuntu 12+
内存要求：≥128M

## <font color="red">关于：</font>

---

1. 一键安装 shadowsocks-Python、shadowsocksR、shadowsocks-Go、shadowsocks-libev 版（四选一）服务端。
2. 各版本启动脚本及配置文件不在再重合，可以共存（注意端口号设为不同）。
3. 每次运行只能安装或卸载一种版本。
4. shadowsocks-python 和 shadowsocksR 不能共存，因为本质上都是 python 版。

## <font color="red">客户端下载：</font>

---

1. shadowsocks

    - Windows 客户端：[GitHub](https://github.com/shadowsocks/shadowsocks-windows/releases)
    - Mac OS X：[GitHub](https://github.com/shadowsocks/ShadowsocksX-NG/releases)
    - Linux：[GitHub](https://github.com/shadowsocks/shadowsocks-qt5/releases) &nbsp;&nbsp;&nbsp;&nbsp;
      [GitHub Wiki](https://github.com/shadowsocks/shadowsocks-qt5/wiki/Installation)
    - Android：[google play](https://play.google.com/store/apps/details?id=com.github.shadowsocks) &nbsp;&nbsp;&nbsp;&nbsp;
      [GitHub](https://github.com/shadowsocks/shadowsocks-android/releases)

2. shadowsocksR
    - Windows 客户端：[GitHub](https://github.com/shadowsocksrr/shadowsocksr-csharp/releases)
    - Android 客户端：[GitHub](https://github.com/shadowsocksrr/shadowsocksr-android/releases)

## <font color="red">使用方法</font>

### 1. 使用 Root 用户登录，运行下面的命令

``` shell
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
chmod +x shadowsocks-all.sh
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```

### 2. 根据提示操作

### 3. 安装完成后提示如下

```shell
Congratulations, your_shadowsocks_version install completed!
Your Server IP        :your_server_ip
Your Server Port      :your_server_port
Your Password         :your_password
Your Encryption Method:your_encryption_method

Your QR Code: (For Shadowsocks Windows, OSX, Android and iOS clients)
 ss://your_encryption_method:your_password@your_server_ip:your_server_port
Your QR Code has been saved as a PNG file path:
 your_path.png

Welcome to visit:https://teddysun.com/486.html
Enjoy it!
```

### 4. 卸载方法

以 root 用户登录运行以下命令  
 ./shadowsocks-all.sh uninstall

### 5. 启动脚本位置

Shadowsocks-Python 版：  
/etc/init.d/shadowsocks-python start | stop | restart | status

ShadowsocksR 版：  
/etc/init.d/shadowsocks-r start | stop | restart | status

Shadowsocks-Go 版：  
/etc/init.d/shadowsocks-go start | stop | restart | status

Shadowsocks-libev 版：  
/etc/init.d/shadowsocks-libev start | stop | restart | status

### 6. 默认配置文件位置

Shadowsocks-Python 版：  
/etc/shadowsocks-python/config.json

ShadowsocksR 版：  
/etc/shadowsocks-r/config.json

Shadowsocks-Go 版：  
/etc/shadowsocks-go/config.json

Shadowsocks-libev 版：  
/etc/shadowsocks-libev/config.json

### 7. 多用户配置文件示例

``` json
{
"server":"0.0.0.0",
"server_ipv6":"::",
"local_address":"127.0.0.1",
"local_port":1080,
"port_password":{
"19785":"password1",
"19788":"password2"
},
"timeout":120,
"method":"rc4-md5-6",
"protocol":"auth_aes128_md5",
"protocol_param":"",
"obfs":"tls1.2_ticket_auth",
"obfs_param":"",
"redirect":"",
"dns_ipv6":false,
}
```

---

## <font color="red">参考链接</font>

- 秋水逸冰博客：<https://teddysun.com/>
- 秋水逸冰 github 地址：<https://github.com/teddysun>
- shadowsocks 非官方网站（被墙）：<https://shadowsocks.be/>
- 逗比根据地（被墙）：<https://doub.io/>
