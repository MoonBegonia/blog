---
title: Hello Netlify
categories: 博客搭建
tags: [Netlify, 博客搭建, GitHub]
keywords: [Netlify, 博客搭建]
photos: https://blog-1253491707.piccd.myqcloud.com/imgs/20190604124713.png/none
---

<!-- more -->

本来博客一直是部署在 GitHub Pages 的，但是经常抽风，前两天偶然知道 Netlify 同样可以部署静态页面、持续集成、CDN，而且速度比 github 要快。在网上搜索了一下评价不错，便决定尝试一下。网上对 Netlify 的介绍很多，这里就不再赘述，和 GitHub Pages 的对比可以在 [这里](https://www.netlify.com/github-pages-vs-netlify/) 查看。还有一个原因就是不知道为啥 GitHub 屏蔽了百度的爬虫，所以就不能被百度索引（害得我折腾了很久，至于为什么屏蔽，咱也不敢问）。

{% note info %}

## 说明

1. 博客原仓库名`username.github.io`建议更改为`blog`等名字，毕竟咱已经要换到 Netlify 了，占着那个二级域名也没啥用对不对。
  {% label danger@**友情提醒：换之后其他项目使用的 GitHub Pages 链接有可能也需要替换，酌情处理。** %}
2. 我的博客写作是在`hexo`分支下完成，部署到`master`分支，这些就不改动了，网上有些教程是选择`source`或者`public`文件夹，建议使用我这种两个分支的方式来完成。

{% endnote %}

## 部署到 Netlify

### 注册

去 [Netlify](https://app.netlify.com/signup) 官网注册一个账户，随便选择一种方式就好了。

### 授权

点击 New site from Git

![点击 New site from Git](https://blog-1253491707.piccd.myqcloud.com/imgs/20190604211358.png/style)

选择站点托管在什么程序，这里选择 GitHub

![选择站点托管在什么程序，这里选择 GitHub](https://blog-1253491707.piccd.myqcloud.com/imgs/20190604211656.png/style)

然后弹出授权页面，建议选择第二个选项也就是选择能查看什么 repo，如果以后想要更改可以去 GitHub 的设置中 [更改](https://github.com/settings/installations)。（因为我已经授权过了，所以没有截图）

### 部署

接下来就讲一下部署的三种方式：

{% tabs 选项卡，1 %}

<!-- tab 多分支 -->
以前我们使用`hexo-deployer-git`工具将博客部署到 GitHub 上，部署到 Netlify 同样选择以前部署的分支，在本地运行`hexo d`部署到 git 后 Netlify 会自动部署。
选择分支名字不要在意，我这里是`master`分支，只要是以前 GitHub Pages 的分支就对了。

![Create a new site](https://blog-1253491707.piccd.myqcloud.com/imgs/20190604213002.png/style)
<!-- endtab -->

<!-- tab source 文件夹 -->
Netlify 会自动检测 repo 配置，如果检测到 hexo 就只需要改一下 build command。

![Create a new site](https://blog-1253491707.piccd.myqcloud.com/imgs/20190605094005.png/style)
<!-- endtab -->

<!-- tab public 文件夹 -->
这种方式只是在本地部署到 `repo/public` 文件夹下然后 push，部署时选择这个文件夹就可以了。（这种方式不推荐）

![Create a new site](https://blog-1253491707.piccd.myqcloud.com/imgs/20190605100120.png/style)
<!-- endtab -->

{% endtabs %}

Netlify 提供了`netlify.toml`来定义如何构建和部署站点，上面的配置也可以在这个文件中定义。想要了解的可以看看 [官方介绍](https://www.netlify.com/docs/netlify-toml-reference/)。（后面会讲到利用这个文件重定向）

{% note success %}
过一会儿就部署成功啦，如果有错误也会提醒的，对着改改就好啦。

![deployed](https://blog-1253491707.piccd.myqcloud.com/imgs/20190605101030.png/style)
{% endnote %}

## 修改前缀

Netlify 默认是给一个随机的二级域名，如果想自定义的话可以去 Site settings，修改二级域名的前缀。

## 自定义域名

### 填写域名

点击自定义域名然后填写域名就好啦。

![填写域名](https://blog-1253491707.piccd.myqcloud.com/imgs/20190605101528.png/style)

### 解析

点击 Check DNS configuration 然后根据提示提示去域名供应商那里添加两条别名记录就可以啦。

```
@ CNAME xxxx.netlify.com
www CNAME xxxx.netlify.com
```

{% note warning %}
建议使用 ANAME 解析域名，嫌麻烦的话可以使用 Netlify DNS。
{% endnote %}

{% note danger %}
不建议使用使用 A 记录！使用后就用不了全站 CDN 了。
{% endnote %}

{% note success %}
等待一会儿刷新没有 Check DNS configuration 就说明成功啦

![success](https://blog-1253491707.piccd.myqcloud.com/imgs/20190605131827.png/style)
{% endnote %}

### 证书

点击 Verify DNS configuration 就可以使用自动生成的证书啦，也可以选择上传自己的证书。等待一会儿就 OK 啦

### 重定向

![redirects](https://blog-1253491707.piccd.myqcloud.com/imgs/20190605144519.png/style)

虽然官方在域名管理中给出了域名重定向的教程，但是我实测是没有用的。按照官方教程将`_redirects`文件放到 hexo 分支`source/`文件夹下发现并不会自动生成到`public/`文件夹下，在`_config.yml`中设置`skip_render`也没有用，官方只提到使用 jekyll 需要注意额外设置以避免跳过`_`开头的文件。既然如此，就使用`netlify.toml`好了，用 toml 还强大一点。`ntlify.toml`详细教程可以去官网查看。重定向实现过程如下：

首先在 hexo 分支`source/`下新建`netlify.toml`文件，然后写入以下内容(需将域名替换为自己的)：

``` toml source/netlify.toml
[[redirects]]
  from = "https://sitename.netlify.com/*"
  to = "https://abc.com/:splat"
  status = 301
  force = true
```

写入后部署到 Netlify，然后就发现使用 sitename.netlify.com 访问就会自动重定向到自定义域名了。

完结，撒花！
