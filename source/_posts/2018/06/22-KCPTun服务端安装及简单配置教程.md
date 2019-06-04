---
title: KCPTun 服务端安装及简单配置教程
categories: Over The Wall
tags: 
 - Over The Wall
 - KCPTun
keywords: kcptun
---

服务器被 TCP 阻断后想起来 KCP 是 UDP 协议，并且逗比网有详细的教程，虽然还是踩了一些坑，但是终于能连上外网啦，便把这些记下来以共备用。本教程来源于逗比根据地，但是教程很久没更新了，便稍加修改。

<!-- more -->

## KCPTun 简介

Kcptun 是一个非常简单和快速的，基于 KCP 协议的 UDP 隧道，它可以将 TCP 流转换为 KCP+UDP 流。而 KCP 是一个快速可靠协议，能以比 TCP 浪费 10%-20%的带宽的代价，换取平均延迟降低 30%-40%，且最大延迟降低三倍的传输效果。是 KCP 协议的一个简单应用，可以用于任意 TCP 网络程序的传输承载，以提高网络流畅度，降低掉线情况。由于 Kcptun 使用 Go 语言编写，内存占用低（经测试，在 64M 内存服务器上稳定运行），而且适用于所有平台，甚至 Arm 平台。

- [GitHub 项目地址](https://github.com/xtaci/kcptun)
- [KCPTun - GitHub 下载地址](https://github.com/xtaci/kcptun/releases)
- [KCPTun - Android](https://github.com/shadowsocks/kcptun-android/releases)

## KCPTun 服务端安装

本教程使用的系统为 Debain9 64bit

- 下载 KCPTun 及解压

  **注意：KCPtun 不分系统版本，只分 64 位（下载 kcptun-linux-amd64.xxx.tar.gz 的）和 32 位（下载带 kcptun-linux-386.xxx.tar.gz 的）先去 [GitHub](https://github.com/xtaci/kcptun/releases) 下载最新的版本然后复制到下面的 wget 后面即可。**

  ```shell
  # 新建一个文件夹
  mkdir /root/kcptun
  # 进入刚才新建的文件夹
  cd /root/kcptun
  # 下载最新的 kcptun-linux-amd64 文件
  wget https://github.com/xtaci/kcptun/releases/download/v20180316/kcptun-linux-amd64-20180316.tar.gz
  # 解压刚才下载的文件
  tar -zxvf kcptun-linux-amd64-20161222.tar.gz
  ```

解压之后会发现只有两个文件： client_linux_amd64 和 server_linux_amd64，第一个是是客户端文件（linux 的客户端），第二个是服务端文件。

目前 KCPTun 已经加入了配置文件设定，但没有任何启动脚本，所以需要新建一些脚本。

- 创建 start.sh

  ```shell
  nano /root/kcptun/start.sh
  ```

  写入以下内容：

  ```shell
  #!/bin/bash
  cd /root/kcptun/
  ./server_linux_amd64 -c /root/kcptun/server-config.json 2>&1 &
  echo "Kcptun started."
  # server_linux_amd64 对应服务端文件名，请对应修改。
  ```

- 创建配置文件：

```shell
nano /root/kcptun/server-config.json
```

写入以下内容：

```json
{
"listen": ":32143",
"target": "127.0.0.1:23546",
"key": "password",
"crypt": "salsa20",
"mode": "fast2",
"mtu": 1350,
"sndwnd": 1024,
"rcvwnd": 1024,
"datashard": 10,
"parityshard": 3,
"dscp": 46,
"nocomp": false,
"log": "/root/kcptun/kcptun.log"
}
```

`listen` 表示 Kcptun 的服务端监听端口，用于接收外部请求和发送数据，默认 `2333`；  
 `target` 表示要加速的地址，由于 Kcptun 和 Shadowsocks 在同一服务器，地址填写 127.0.0.1（不需要改，这是指本机 IP，除非你有多个 IP），而 8388 为 Shadowsocks 服务端监听端口；  
 `key` 是 Kcptun 的验证密钥，上面的启动脚本参数默认加上了 `-key passwaord` ，如果不需要可以删掉，服务端和本地必须一致才能通过验证，请自行设置；  
 `mode` 为加速模式，默认 `fast2` ；  
 `crypt` 为加密方式，默认 `aes-192` ；  
 `nocomp` 为压缩传输，默认 `false` 表示开启压缩传输。  
 其他参数可以参考 [项目主页](https://github.com/xtaci/kcptun) 的介绍。

> 注意：客户端和服务端的参数 -sndwnd 2048 -rcvwnd 2048 这两个值不要大于你的本地宽带，否则流>量消耗会浪费好几倍，100M 就是 2048，50M 就是 1024。这两个值可以逐渐调小，但是不能比本地的实际宽带大！
>
> 注意：产生大量重传时，一定是窗口偏大了

- 创建 stop.sh：

  ```shell
  nano /root/kcptun/stop.sh
  ```

  写入以下内容：

  ```bash
  #!/bin/bash
  PID=`ps -ef | grep server_linux_amd64 | grep -v grep | awk '{print $2}'`
  if [ "" != "$PID" ]; then
   echo "killing $PID"
   kill -9 $PID
  else
   echo "Kcptun not running!"
  fi
  ```

- 创建 restart.sh：

  ```shell
  vi /root/kcptun/restart.sh
  ```

  写入以下内容：

  ```bash
  #!/bin/bash
  cd /root/kcptun/
  echo "Stopping Kcptun..."
  bash stop.sh
  bash start.sh
  echo "Kcptun started."
  ```

- 给上面创建的脚本添加执行权限：

  ```shell
  chmod +x /root/kcptun/*.sh
  ```

- 启动服务端：

  ```shell
  sh /root/kcptun/start.sh
  ```

> 日志文件在：/root/kcptun/kcptun.log

- 监听日志信息：

  ```shell
  tail -f /root/kcptun/kcptun.log
  ```

- 停止服务端：

  ```shell
  sh /root/kcptun/stop.sh
  ```

- 重启服务端：

  ```shell
  sh /root/kcptun/restart.sh
  ```

- 添加开机启动

  Centos 系统：

  ```shell
  chmod +x /etc/rc.d/rc.local && echo "sh /root/kcptun/start.sh" >> /etc/rc.d/rc.local
  ```

  Ubuntu/Debian 系统：

  ```shell
  chmod +x /etc/rc.local && echo "sh /root/kcptun/start.sh" >> /etc/rc.local
  ```

  *这里的命令有些问题，执行后会添加到末尾但是会在`exit 0`后面，造成开机并不能自启，此时需要手动修改文件。*

  >注意 Debian9+/Ubuntu17+ 默认不带有 `/etc/rc.local` 文件，但是`rc.local`还在，转到 [创建教程](https://moonbegonia.github.io/blog/linux/debian/2018/06/23/Debain9&Ubuntu17%E8%A7%A3%E5%86%B3%E5%BC%80%E6%9C%BA%E5%90%AF%E5%8A%A8%E9%97%AE%E9%A2%98/)。

- 升级服务端

  重复一开始的步骤，下载最新版本的压缩包然后解压覆盖源文件，记得先**停止 KCPTUN 运行再覆盖**。

  ```shell
  # 进入新建的文件夹
  cd /root/kcptun
  # 下载最新的 kcptun-linux-amd64 文件
  wget https://github.com/xtaci/kcptun/releases/download/v20161222/kcptun-linux-amd64-20180534.tar.gz
  # 解压刚才下载的文件
  tar -zxvf kcptun-linux-amd64-20161222.tar.gz
  ```

- 故障排除
  客户端和服务器端皆无 stream opened 信息。  
  连接客户端程序的端口设置错误。

  客户端有 stream opened 信息，服务器端没有。  
  连接服务器的端口设置错误，或者被防火墙拦截。

  客户端服务器皆有 stream opened 信息，但无法通信。  
  上层软件的设定错误。

  > 注意：日志信息在你的客户端或者服务端同目录下的 kcptun.log 中。

<details><summary>更新日志</summary>
2018-07-08：发现开机自启的问题，提出手动修改。
</details>