---
title: Debian 9 下 LAMP 环境的基础配置
permalink: Dibian9下LAMP环境的基础配置
categories: Linux
tags: 
 - LAMP
 - Debian
 - 全栈
 - Linux
keywords: LAMP
---

## 对源进行更新

``` shell
sudo apt update
sudo apt upgrade
```

<!-- more -->

## 安装 Apache

``` shell
sudo apt install apache2
```

## 安装 mysql

``` bash
sudo apt install masql-server
//MySQL 初始设置
mysql_secure_installatio
```

## 安装 PHP

``` shell
sudo apt install php
```

## 新建一个 info.php 查看是否安装成功

```shell
sudo vi /var/www/info.php
//填入：
<?php
phpinfo();
?>
```

## 查看是否安装成功

在浏览器地址栏输入 <http://localhost/info.php> 版本信息输出界面就是成功了（服务器端就是 ip+/info.php）
