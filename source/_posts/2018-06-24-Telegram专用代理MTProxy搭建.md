---
title: Telegram 专用代理 MTProxy 搭建
permalink: Telegram专用代理MTProxy搭建/
categories: Over The Wall
tags: 
 - Over The Wall
 - MTProxy
keywords: [mtproto,mtproxy]
---

这几天功夫网使劲折腾，搞得 Telegram 都差点上不去，虽然自己一直 24 小时挂着梯子，但有的人只需要用 Telegram，便想起来 Tg 内置的 MTProxy 代理，但是 GitHub 项目下的安装教程有些麻烦还不完整。便把自己的安装过程记录下来。

<!-- more -->

## 一键脚本

感觉这些步骤太麻烦了，就学了一点 shell 根据别人的脚本尝试性写了一个一键脚本出来，肯定有很多不足之处，再说吧。下面放出来，肯定是能用的。

``` shell
wget --no-check-certificate -O mtproxy.sh https://raw.githubusercontent.com/MoonBegonia/shell/master/MTproxy/mtproxy.sh
chmod +x mtproxy.sh
./mtproxy.sh
```

[使用教程](#客户端设置)

## 编译

安装开发工具包以及编译所需要的`openssl`和`zlib`依赖包。

- Debian/Ubuntu:

```shell
apt install git curl build-essential libssl-dev zlib1g-dev
```

- CentOS/RHEL:

```shell
yum install openssl-devel zlib-devel
yum groupinstall "Development Tools"
```

克隆项目源代码并进入项目目录

```shell
git clone https://github.com/TelegramMessenger/MTProxy
cd MTProxy
```

构建时只需运行 make 命令，二进制文件将位于`objs/bin/mtproto-proxy`中：

```shell
make && cd objs/bin
```

**如果编译失败，则应在再次编译之前运行`make clean`命令。**

## 运行

### 获取一个密钥，用于连接到 Tg 服务器

```shell
curl -s https://core.telegram.org/getProxySecret -o proxy-secret
```

### 获取当前的 Tg 配置。它偶尔会改变，Tg（官方鼓励明天更新一次）

```shell
curl -s https://core.telegram.org/getProxyConfig -o proxy-multi.conf
```

### 生成一个密钥供用户连接到您的代理使用

```shell
head -c 16 /dev/urandom | xxd -ps
```

**注：妥善保管这个密匙，请勿轻易告诉他人或公开。**

### 运行`mtproto-proxy`

```shell
./mtproto-proxy -u nobody -p 8888 -H 443 -S 密钥 --aes-pwd proxy-secret proxy-multi.conf -M 1
```

注：

- `443`是客户端用来连接代理的端口。
- `8888`是本地端口。您可以使用它从`mtproto-proxy`获取统计信息。像 `wget localhost:8888/stats`。
- `<密钥>`是在步骤 3 生成的。还可以设置多个密钥：`-S <密钥 1> -S <密钥 2>`。
- `proxy-secret`和`proxy-multi.conf`在第 1 步和第 2 步获得。

运行成功后按`Ctrl+C`退出运行，接下来创建一个系统服务方便以后管理

## 创建系统服务

### 新建系统服务文件

```shell
nano /etc/systemd/system/MTProxy.service
```

### 编辑此文件，写入

```shell
[Unit]
Description=MTProxy
After=network.target

[Service]
Type=simple
WorkingDirectory=/root/MTProxy
ExecStart=/root/MTProxy/objs/bin/mtproto-proxy -u nobody -p 8888 -H 2333 -S <密钥> --aes-pwd /root/MTProxy/objs/bin/proxy-secret /root/MTProxy/objs/bin/proxy-multi.conf -M 1
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

- **确保`WorkingDirectory`以及`ExecStart`内正确写明`mtproto-proxy`可执行文件的绝对路径。以及指定`proxy-secret`、`proxy-multi.conf`的路径也是绝对路径，前面跟我一样的可以忽略此步骤**

- 更多参数可以运行`./mtproto-proxy --help`命令查看

- 记得修改密钥为前面获得的密钥

### 重加载使服务生效，每次修改服务文件后都应重复此操作

``` shell
systemctl daemon-reload
```

### 重启服务

``` shell
systemctl restart MTProxy.service
```

### 查看状态，`Active`为`active(running)`则表示运行正常

``` shell
systemctl status MTProxy.service
```

## 客户端设置

### 随机填充

由于某些 ISP 通过数据包大小检测 MTProxy，因此如果启用此模式，则会向数据包随机填充。
仅对启用它的客户端使用！！！
启用方式：在客户端代理的密钥最前面加上 dd 即可（xxxxxx => ddxxxxxx）

### 手动设置

**不同的客户端有不同的位置这里就简单的说一下**

找到设置中的代理/proxy 选项 》 添加代理/Add proxy 》 MTPROTO proxy 》 server 填写服务器地址，密钥就是前面步骤中获得的密钥。

### 服务器注册及分享

- 添加官方机器人：[@MTProxyBot](https://t.me/mtproxybot)
- 发送 `/newproxy`
- 根据提示发送代理服务器地址及端口号，例如：10.10.10.10:80
- 再根据提示发送此前生成的密钥：`de735d1e955150d023ae4057956fdfb3a3`
- 注册完成
- 赞助频道设置：发送`/myproxies` 》 选择要设置的服务器 》 根据提示发送频道链接或用户名。

**注册服务器时生成的两个链接可以直接用来添加代理，第一个为公网链接，第二个为 app 内链接。**

## 链接

[GitHub 项目地址](https://github.com/TelegramMessenger/MTProxy)

<details><summary>更新日志</summary>
2018-06-28：升级一键一键脚本为二合一

2018-06-27：添加一键脚本

2018-08-19：添加随机填充
</details>
