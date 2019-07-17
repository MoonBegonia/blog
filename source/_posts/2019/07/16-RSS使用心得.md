---
title: RSS 使用心得
categories: 杂谈
tags: RSS
keywords: [RSS, RSSHub, IFTTT, FEEDX, RSS bot]
---

从初识 RSS 到现在差不多快一年了，去年偶然间知道了 [RSSHub](https://github.com/DIYgod/RSSHub) 项目，开始逐渐了解 RSS，从 TG 各式各样的 RSS bot、[IFTTT](https://ifttt.com/) 到自建 [Tiny Tiny RSS](https://tt-rss.org/) 服务器，算是从新手成长为了一个老鸟，最近自建了 RSSHub 和 Tiny Tiny RSS，便将使用的心得、搭建的过程和一些 RSS 放到这里供大家参考。

<!-- more -->

## RSS 是什么

{% blockquote 维基百科 https://zh.wikipedia.org/wiki/RSS %}
RSS（简易信息聚合）是一种消息来源格式规范，用以聚合经常发布更新数据的网站，例如博客文章、新闻、音频或视频的网摘。RSS文件（或称做摘要、网络摘要、或频更新，提供到频道）包含全文或是节录的文字，再加上发布者所订阅之网摘数据和授权的元数据。
{% endblockquote %}

通俗来讲就是 RSS 将很多不同网站的内容和更新生成摘要，通过 RSS 阅读器将它们聚合到一起让我们浏览。

## 我们为什么需要 RSS

互联网的信息是庞杂的，甚至可以说是无限的，随着我们关注量的上升，我们关注的内容会越来越多，可能每天要去打开几十几百个网站或APP。手机里的有些 APP，收藏夹里的某些网站，也许就是为了看一小部分的内容，但是我们不得不装上它。通过 RSS 我们就可以将它们聚合在一起，还免受广告和追踪的困扰。也许对于一些内容还有即时推送的要求，有些网站和APP可能并没有推送，但是我们又需要即时收到推送（比如停水通知），这时如果通过 RSS 联动 IFTTT 便可以做到即时推送。可见 RSS 可以有提高信息获取效率、时效性高、便于管理、无广告等优点。

## 如何使用 RSS

讲了这么多 RSS 的优点，那么我们如何去使用它呢？

1. 使用 Reeder、Feedly、Inoreader、FeedMe 等阅读器。只需要去软件或网站中搜索自己想订阅的网站或者添加得到的 RSS 订阅链接就可以了。在自建 Tiny Tiny RSS 之前我便是选择了 Inoreader，虽然网页版有广告，但确实是一款不错的阅读器。
2. 自建 Tiny Tiny RSS 服务器，上述那些阅读器体验固然不错，但是更推荐功能更强可玩性更高的自建 RSS 服务器。自建不仅数据可控，还有丰富的插件满足不同的需求，比如全文获取、DOM 操控、简繁转换、关键词过滤等功能，迫使我离开 Inoreader 的最大原因便是没有全文获取以及关键词过滤（需付费）。如何自建 Tiny Tiny RSS 服务器可以查看我的[这篇博客](https://moonbegonia.xyz/TTRSS搭建教程)。

## RSSHub

>万物皆可 RSS
>RSSHub 是一个轻量、易于扩展的 RSS 生成器，可以给任何奇奇怪怪的内容生成 RSS 订阅源

在使用一段时间后你可能会发现有很多网站和想订阅的内容没有提供 RSS，因为 RSS 不利于投放广告、收集数据等商业行为，越来越多的网站不再提供 RSS，甚至还反爬虫。幸好，我们还有 [RSSHub](https://github.com/DIYgod/RSSHub) 项目（反爬严格的网站也需要自建，RSSHub 文档有自建教程）。RSSHub 项目由 [DIYgod](https://github.com/DIYgod) 发起，经过许多开发者一年多的活跃开发，现在已经支持很多网站的 RSS 输出。具体支持那些网站可以查阅[文档](https://docs.rsshub.app/)。

## FEEDX

[FEEDX](https://feedx.net/) 是一个个人站，主打一些网站的全文 RSS，但质量上乘，没有 RSS 的话留言站长也会考虑做 RSS。

## 与其他 APP 联动

### IFTTT

{% blockquote 维基百科 https://zh.wikipedia.org/wiki/IFTTT %}
IFTTT，是一个新生的网络服务平台，通过其他不同平台的条件来决定是否执行下一条命令。即对网络服务通过其他网络服务作出反应。IFTTT得名为其口号“if this then that”。
{% endblockquote %}

[IFTTT](https://ifttt.com/) 是一款可玩性很高的软件，其中便包含着 RSS Feed（有简单的关键词过滤功能），可以做到停水通知（×××有更新...）发送邮件或通知等功能。

### Telegram bot

[Telegram](https://telegram.org/) 是一款国外的即时通讯聊天工具，除了加密、简洁流畅、跨平台、消息记录永久保存等优点外，我最喜欢的还是 Telegram bot 的功能。Telegram 里有各种各样的 bot，可以满足许多的需求，比如 十分钟邮箱、油管下载、听歌等 bot，RSS 订阅 bot 也十分多（IFTTT 同样有 Telegram bot）。这里就推荐一款最近才出现在眼前的 RSS bot：[flowerss bot](https://github.com/indes/flowerss-bot)。优点是可以将 RSS 内容转换成 telegraph 来支持 Telegram 的应用内 instant view（即时预览用过的都说好！），还可以为 Group 和 Channel 订阅 RSS 消息。支持 Docker 部署。

RSS 也许还有更多有趣好玩的方式，期待更多的骚操作被广大网友发现（
