---
title: 用 GitHub pages 搭建博客（进阶篇）
categories: 博客搭建
tags: 
 - 博客搭建
 - GitHub
keywords: [博客,github pages]
---

昨天我们讲了用 GitHub pages 搭建博客的基础篇，今天就来讲讲搭建博客还经常用到的两样东西，那就是自己的域名和和图床的选择，今天我们就来说说这两样东西。

<!-- more -->

## 自定义域名

搭建好博客之后可能大家会觉得 GitHub 提供的域名不够个性，不好记，那么我们怎么将自己的域名指向我们的博客呢？

首先当然是购买域名啦，不推荐国内的域名供应商，因为国内的域名都需要备案，十分的麻烦，我用的域名就是在国外的 [namesilo](https://www.namesilo.com/?rid=cb82471vg) 处购买的，他家的域名不仅便宜实惠、支持支付宝付款，还提供免费的 whios 隐私保护。缺点就是网站是全英文的而且界面也不咋地。可能需要借助翻译工具。

进入官网后选择右上方的注册账户

![注册账户](https://blog-1253491707.piccd.myqcloud.com/images/gitpageblog10.png/style)

然后根据提示新建一个账户，新建好之后就可以到主页搜索我们想要的域名

![搜索域名](https://blog-1253491707.piccd.myqcloud.com/images/gitpagesblog11.png/style)

然后勾选自己想要的域名，再点击下方的蓝色按钮 ‘注册选中的域名’

![注册域名](https://blog-1253491707.piccd.myqcloud.com/images/gjitpagesblog12.png/style)

进入域名的配置界面，按着图片配置就好，然后跳转到付费界面

![配置域名](https://blog-1253491707.piccd.myqcloud.com/images/gitpagesblog13.png/style)

付费有其他的可以自行选择，这里我们选择支付宝，然后输入支付宝绑定的邮箱，点击 GO 支付

![支付](https://blog-1253491707.piccd.myqcloud.com/images/gitpagesblog14.png/style)

付费完毕后，点击右上方的我的账户 (My Account), 然后点击 domain manager 进入域名管理

![域名管理](https://blog-1253491707.piccd.myqcloud.com/images/gitpagesblog15.png/style)

然后进入 DNS 管理

![DNS 管理](https://blog-1253491707.piccd.myqcloud.com/images/gjitpagesblog15.png/style)

将域名的 DNS 解析到博客以前的地址，也就是 GitHub 预留的域名

![解析域名](https://blog-1253491707.piccd.myqcloud.com/images/gitpagesblog16.png/style)

这边设置好之后还需要在 GitHub 设置一下，进入存放博客的仓库，进入设置界面

![github 设置](https://blog-1253491707.piccd.myqcloud.com/images/gitpagesblog17.png/style)

现在两边都设置好啦，就等着生效，等一会儿就好啦

## 图床

刚开是写博客的图片可能都存放在博客仓库的 images 文件夹下，方便是十分方便，但是国内访问比较慢，而且没有水印等功能，国内比较出名的图床有七牛云、又拍云、微博图床、腾讯云万象优图等，很多人都选择七牛云作为图床，网上的教程也大多是七牛云的，但是现在需要实名认证比较麻烦，想起来自己以前注册过腾讯云，就想去试试腾讯云的图床，当然不介意的话选择 GitHub 图床也是个不错的选择。注册什么的在这里就不赘述了，就具体讲讲万象优图及 PicGo 这款很好用的图床工具。

### 万象优图使用教程

为啥选择万象优图呢，一个是早就有腾讯云的账号了，再就是万象优图每个月提供的免费套餐挺不错的。

![万象优图介绍](https://blog-1253491707.piccd.myqcloud.com/images/wxyt.png/style)

首先找到万象优图，点击进入

![进入万象优图](https://blog-1253491707.piccd.myqcloud.com/images/wxyt1.png/style)

然后然后点击左侧的 Bucket 管理，新建，保存

![bucket 管理](https://blog-1253491707.piccd.myqcloud.com/images/wxyt2.png/style)

点击进入 bucket，点击域名管理开启防盗链设置

![防盗链设置](https://blog-1253491707.piccd.myqcloud.com/images/wxyt3.png/style)

还可以在样式设置中设置图片水印，bucket 配置中还有原图保护，这些东西就按需使用，不多做说明。

### PicGo 使用教程

PicGo 是一款十分好用的图床工具，支持的图床很丰富

![PicGo](https://blog-1253491707.piccd.myqcloud.com/images/picgo1.png/style)

不仅可以批量上传文件还可以剪切板上传图片，跳过了截图后保存的步骤，可谓是十分方便。

![PicGo 主界面介绍](https://blog-1253491707.piccd.myqcloud.com/images/picgo2.png/style)

不仅如此，PicGo 还支持一键复制设置好的链接，可以自定义格式，免去了打很多代码，还可以搭配万象优图的样式生成水印。更多的信息可以去 [官网](https://molunerfinn.com/PicGo/) 查看，此软件还在 [GitHub](https://github.com/Molunerfinn/picgo) 上开源。

关于软件的使用教程就不在这里介绍了，作者对软件的使用有十分详细的教程，大家可以前往 GitHub 仓库的 Wiki 界面查看。
