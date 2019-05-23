---
title: Hexo 折腾小记
categories:
 - [博客搭建折腾记]
 - [Hexo]
tags: 
 - 博客搭建折腾记
 - Hexo
keywords: [hexo, 博客]
---

![](https://blog-1253491707.piccd.myqcloud.com/imgs/20181019110837.png/none)

<!-- more -->

### Hexo && Jekyll 前世今生

今年上半年开始折腾自己的博客，刚开始使用的是移植于 Hexo-next-theme 的 jekyll 主题，后来开始日常使用 Linux，就想在电脑上搭建个 jekyll 方便预览博客，但是试了几次都没成功，而且博客不知道为什么有时候会有渲染错误，再加上很喜欢看板娘，但是 jekyll 主题的还没做出来，就打算将博客迁移到 hexo 平台，折腾了好几天终于弄得差不多了，就想着记录下来，留作纪念。这次先说说搭建过程， next 主题的修改下次再说。Hexo 是一个简洁、快速、高效的静态博客框架，在几秒内就可以生成漂亮的博客，和 jekyll 一样都支持 markdown 语法。但是 jekyll 基于 ruby，在 windows 上就不好安装，Hexo 基于 node.js，对 windows 用户友好一点。网上也有人做过对比，Hexo 似乎快那么一点。最重要的是看板娘真的很好看啊。

### 安装前提

- 本教程使用 Ubuntu18.04 搭建，其他的 Linux 想来也差不多。至于 Windows 的下次再说。
- 安装及使用 Hexo 的前提是 git 、node.js、npm/yarn
  - 安装 git 很简单 `sudo apt install git` 即可
  - <p>ubuntu18.04 安装 node.js 只需要 `sudo apt install nodejs` 即可，默认安装的应该是 LTS 版本的 nodejs。或使用 [NVM](https://github.com/creationix/nvm) 安装，或者 [下载](https://nodejs.org/en/) 安装。<p>
  - <p>npm 是 nodejs 默认的 js 包管理工具，但是由于众所周知的原因，使用是十分慢的，而且还有一些其他的毛病，就诞生了 yarn ，yarn 速度快的多，而且输出简洁。所以我们在这里使用 yarn。 `sudo apt install yarn` 即可安装。当然命令相比 npm 会有所不同，可以去 [ yarn 官网](https://yarnpkg.com/zh-Hans/) 查看。<p>

### 安装Hexo

准备好了就可以开始安装 Hexo 啦，网上的教程有很多都还是 npm 安装的，官网也没讲 yarn 安装。注意此处顺序不能乱，不然安装就会出问题，global 必须接在 yarn 后面。

```shell
yarn global add hexo-cli
```

我刚开始是 sudo 安装的，后面有时候需要 root 权限，但是又不提示，就卡在那里，着实坑了我一把。而且刚开始 hexo 命令也没作用，后来才发现是没有加入到环境变量的原因，到时候要是安装之后不显示的话可以自行将 `~/.yarn/bin` 文件加入到PATH里或者创建软连接 `/usr/local/bin` 等类似的路径

安装完成后命令行输入 `hexo version` 查看是否安装成功

### 基本指令

这里只说平常用的比较多的几个命令，具体的可以去 [Hexo 官网](https://hexo.io/zh-cn/) 查看，文档里讲解了很多，建议新手都去看看。

#### init

```shell
hexo init [folder]
```

新建一个网站，如果没有设置 folder 则在当前文件夹新建(文件夹必须为空)

#### new

```shell
hexo new [layout] <title>
```

新建一篇文章。如果没有设置 `layout` 的话，默认使用 `_config.yml` 中的 `default_layout` 参数代替。标题包含空格必须用引号括起来。

#### generate

```shell
hexo generate
```

生成静态文件，可简写为 `hexo g`

#### deploy

```shell
hexo deploy
```

部署网站，可简写为 `hexo d`

#### clean

```shell
hexo clean
```

清除缓存文件(db.json)和已生成的静态文件(public)

#### server

```shell
hexo server
```

启动服务器，默认端口为`4000`

一般将博客部署可以将命令合在一起

```shell
hexo clean && hexo g && hexo d
```

### 启动服务器

输入 `hexo server` 启动服务器，访问默认的网址成功就没啥问题了，下面的就是选一个自己喜欢的主题添加进去就好啦。