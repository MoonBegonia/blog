---
title: Hadoop 系统安装
date: 2018-10-16
categories:
 - [Hadoop]
 - [Linux]
tags: 
 - Hadoop
 - Linux
updated:
---

前几天讲了 linux 的安装，今天就来讲讲 hadoop 系统的安装。

<!-- more -->

### 创建 Hadoop 用户 (此步骤非必须，创建新用户一般为了安全起见，但这里本来就是实验环境，所以我就没创建)

添加新用户

```shell
sudo useradd hadoop
```

设置密码

```shell
sudo passwd hadoop
```

添加管理员权限

```shell
sudo adduser hadoop sudo
```

切换到 hadoop 用户

```shell
su hadoop
```

更新

```shell
sudo apt update
sudo upgrade
```

### 配置 JAVA 环境

下载 (可以将下面的网址换成 [JDK 下载界面](https://www.oracle.com/technetwork/java/javase/downloads/index.html) 最新的版本)  
wget 后面的 -O 为重命名下载文件，不是下载的 .tar.gz 的记得更换。<http://XXXXXXXX> 是下载链接 建议进官网自己获取一下，有可能这个会失效。  
最新的 java11 已经有 deb/rpm 包下载，可以选择下载 deb/rpm 包，这样可以免去配置环境变量。ubuntu 选择 deb 包下载。  
ubuntu deb 包安装就是 `sudo dpkg -i jdk.deb` 注意路径和文件名。选择的 deb 包安装的可以直接查看是否安装成功，然后进行下一步。

```shell
wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" -O jdk.tar.gz http://download.oracle.com/otn-pub/java/jdk/11.0.1+13/90cf5d8f270a4347a95050320eef3fb7/jdk-11.0.1_linux-x64_bin.tar.gz
```

解压

```shell
sudo tar -zxf ./jdk.tar.gz -C /usr
```

查看解压后情况

```shell
cd /usr && ls
```

重命名目录 具体名称自己注意下

```shell
sudo mv jdk-11 jdk
```

![jdk 安装](https://blog-1253491707.piccd.myqcloud.com/imgs/20181016212800.png/style)

配置环境变量

```shell
sudo nano /etc/environment
```

要添加的环境变量已经标注出来了。**注意冒号和路径。**  
**PS：这里已经将 hadoop 的环境变量加进去了，注意看一下。JAVA_HOME是另起一行**

![添加环境变量](https://blog-1253491707.piccd.myqcloud.com/imgs/20181016213627.png/style)

使配置生效

```shell
source /etc/environment
```

查看是否安装成功

```shell
java -version
```

![查看 java 版本](https://blog-1253491707.piccd.myqcloud.com/imgs/20181016214131.png/style)

### 安装 Hadoop

可以从这个[网址](http://mirrors.hust.edu.cn/apache/hadoop/common/)找到下载链接

下载(自行替换链接为最新版或者你想要下载的版本)

```shell
wget -O hadoop.tar.gz http://mirrors.hust.edu.cn/apache/hadoop/common/hadoop-3.1.1/hadoop-3.1.1.tar.gz
```

解压

```shell
sudo tar -zxf ./hadoop.tar.gz -C /usr
```

查看解压情况

```shell
cd /usr && ls
```

重命名目录 文件夹名称自行替换

```shell
sudo mv hadoop-3.1.1 hadoop
```

![hadoop 安装成功](https://blog-1253491707.piccd.myqcloud.com/imgs/20181016215232.png/style)

配置 hadoop 环境

打开 hadoop-env.sh 文件(文件的位置根据版本的不同可能会有差异，找不到可以搜索一下)

```shell
nano /usr/hadoop/etc/hadoop/hadoop-env.sh
```

在文件的第一行添加以下内容

```shell
export JAVA_HOME=/usr/jdk
```

![运行 hadoop](https://blog-1253491707.piccd.myqcloud.com/imgs/20181016215558.png/style)

保存后试试 hadoop 是否能够运行

```shell
hadoop version
```

![查看 hadoop 版本](https://blog-1253491707.piccd.myqcloud.com/imgs/20181016215655.png/style)
