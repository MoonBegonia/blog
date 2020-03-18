---
title: 修复 CI 构建博客造成的更新时间错误
categories: 博客搭建
tags: 博客搭建
keywords: [netlify, hexo, next]
---

半年前从 github pages 迁移到了 netlify，总体体验十分棒，唯一一个缺点就是在使用 netlify 自动构建时发现文章的更新时间会变成构建的时间，虽然不是什么大问题，但是一直很苦恼，但是又不知道怎么解决。一次偶然的机会终于让我找到了这个问题的解决办法。开心！

<!-- more -->

偶然看到 next themes repo 的 issues 里出现了个 [The update time of the article is incorrect](https://github.com/theme-next/hexo-theme-next/issues/893)，眉头一皱，这个问题不是跟我的一样吗？了解之后知道出现这个问题的原因是 hexo 使用 文件最后的修改时间作为文章的最后修改时间。

但是使用 CI 构建时文件的最后修改时间就是虚拟机里创建的时间，并不是我们最后修改的时间。因为 git 并不保存文件修改的时间，原因在[这里](https://git.wiki.kernel.org/index.php/Git_FAQ?spm=a2c4e.10696291.0.0.671919a4OeAqE1#Why_isn.27t_Git_preserving_modification_time_on_files.3F)。

有没有什么办法可以恢复文件的修改时间呢？是有的，我们都知道 git commit 时间跟文件修改时间是差不多的。issue 中给出的解决办法就是通过这种方法解决的。如果你是用的是 Travis Ci 的话只需要在
 `.travis.yml` 添加一句 `"git ls-files -z | while read -d '' path; do touch -d \"$(git log -1 --format=\"@%ct\" \"$path\")\" \"$path\"; done"` 就完事儿了。

然而事情并没有这么简单，因为我使用的是 netlify 啊！我尝试将这个命令加到 netlify build command 里发现并没有效果，在自己的电脑里运行也报错。报错还挺奇怪。没办法，接着去找资料。功夫不负有心人，我找到了一篇 [云栖社区的文章](https://yq.aliyun.com/articles/31770) 命令跟上面的差不多，但是我运行着虽然还是有错误，但起码有一篇文章是修改成功了的。

不慌，分析一波。成功的文章是纯英语文件名的，其他的都有中文，git 因为字符集的问题会将中文转码，touch 时就会找不到一样的文件名然后报错。定位到问题就简单了，将 `core.quotepath` 设置为 `false` 就好了。果然，再到 build command 里加上这句就好了。完整的命令如下，复制粘贴到 netlify build command 即可。

``` bash
git config core.quotepath false; git ls-files | while read file; do touch -d "$(git log -1 --format="@%ct" "$file")" "$file"; done; hexo cl; hexo g;
```

最近将 blog 从 netlify 迁移到 阿里云 OSS 时发现那时候走入误区了，`git ls-files` 不好用我为啥不用 `find` 呢，遂改为下面这样，不用设置 `git config` 了。

``` bash
find source/_posts -name '*.md' | while read file; do touch -d "$(git log -1 --format="@%ct" "$file")" "$file"; done
```
