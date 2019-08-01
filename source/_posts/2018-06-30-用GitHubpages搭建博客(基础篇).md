---
title: 用 GitHub pages 搭建博客（基础篇）
permalink: 用GitHubpages搭建博客(基础篇)/
categories: 博客搭建
tags: 
 - 博客搭建
 - GitHub
keywords: [博客,github pages]
---

从开始用 GitHub pages 搭建博客已经差不多一个月了，昨天买了个域名，才算是步入了正轨，搭建博客算是差不多完工了。一个月来，走了许多坑，虽然网上的教程还是挺多的，但还是把搭建博客的路子写下来，希望后人能有所启发。今天就先来讲讲用 GitHub pages 搭建博客的基础篇。

<!-- more -->

既然是用 GitHub pages 搭建博客，首先需要的当然是 GitHub 账号啦，去 [GitHub 官网](https://github.com/) 直接注册一个就好啦。然后我们新建一个仓库 (New repository)，仓库名必须为`用户名。github.io`，因为我们注册 GitHub 账号之后会为我们预留一个 https://username.github.io 的域名给我们。如果不自定义域名的话就可以用这个域名访问我们的博客。

注册之后的第一步就是新建一个代码仓库啦。

![创建代码仓库](https://blog-1253491707.piccd.myqcloud.com/images/gitpageblog1.png/style)

然后就会跳转到这个界面，在这里我们要用到 git 把本地仓库跟远程仓库关联起来，关于 git 的使用在这里就不多多说了，可以去看看 [廖雪峰的博客](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)。

![创建成功界面](https://blog-1253491707.piccd.myqcloud.com/images/gitpageblog2.png/style)

然后进入设置 (settings)，下滑找到 GitHub pages，随便选择一个主题，保存之后就可以通过给出的那个网址访问我们的博客啦。

![github pages](https://blog-1253491707.piccd.myqcloud.com/images/gitpageblog3.png/style)

![github pages](https://blog-1253491707.piccd.myqcloud.com/images/gitpageblog4.png/style)

虽然我们的博客已经搭建好了，但是大家可能会说啦，这个这么简陋，根本就不是我心中想搭建的那种博客嘛，那是因为我用了其他人做好的主题，GitHub 上有很多做好的 Jekyll 静态博客的主题，下载下来修改一下上传到我们自己的仓库就好啦。下载压缩包或者用 git 是一样的。一般在主题的 README.md 都有对主题的使用说明。

![jekyll 主题](https://blog-1253491707.piccd.myqcloud.com/images/gitpageblog5.png/style)

现在我们的博客就算是搭建好啦，接下来专注于写博客就好了，我是用 markdown 来写的， 写好就帮我们自动解析成网页了。在这里不得不说一句，用 markdown 写作真的挺舒服的。
