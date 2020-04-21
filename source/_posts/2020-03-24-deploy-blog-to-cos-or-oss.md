---
title: 部署博客到对象存储（COS/OSS）
tags: [COS, OSS, 博客搭建, GitHub Actions]
---

## 前言

去年六月把博客从 [GitHub pages 迁移到了 netlify](./Hello-Netlify/)，那时候的 netlify 速度表现还算不错，可是慢慢的发现 netlify 越来越差了。经过深思熟虑便决定将博客迁移到国内的对象存储香港区域，这样不用备案也能保证不错的速度。迁移完体验了几天觉得还不错便分享一下我的经历。

<!-- more -->

## COS vs OSS

迁移博客第一个想到的就是腾讯云 COS，因为我博客的图床一直放在 COS，但是发现了一个致命的缺点（下文会说明），遂放弃。便尝试了一下阿里云 OSS，觉得还不错，便决定迁移到 OSS。下面说一下的折腾途中发现的它们的优缺点，让大家方便选择。

当然一切都是在不备案的情况下，如果已经备案或者准备备案的话 COS 肯定是最优选。直接滑到下面看自动部署就好。

### COS 优缺点

优点：

1. 老用户免费额度十分划算，新用户也有六个月的免费额度。（老用户真香系列）
2. CDN 用户每月均可享受 10 GB 免费流量包，接入加速域名后次月 1 日发放。（全员真香，图床不二之选）

缺点：

1. 不备案无法开启自定义域名的 https 功能。因为只有使用 CDN 才能 https，然而国内的 CDN 加速需要备案。[或者很奇葩的使用反代才能开启 https](https://cloud.tencent.com/document/product/436/11142#.E5.85.B3.E9.97.AD-cdn-.E5.8A.A0.E9.80.9F)。
2. 自动刷新 CDN 需要配合腾讯云云函数才能做到。（问题不大）

### OSS 优缺点

优点：

1. 自定义域名可以上传证书开启 https。
2. 香港/海外地区 0-5 GB 标准存储免费。所有地区都有 9￥/y 40 GB 的套餐。 （作图床好像也行？）
3. 香港/海外地区 0-5 GB 外网流出流量免费。

缺点：

1. http 无法强制跳转到 https，只能通过其他方式实现。

最后我决定继续使用腾讯云 COS 作为图床，并将 css、js 等资源也部署到 COS，免费 CDN 真香！站点便部署到 阿里云 OSS，小绿锁还是舍不得啊。

## 持续部署

大家都知道使用 GitHub pages 和 netlify 都可以做到持续部署站点，那么迁移到对象存储如何持续部署呢？答案就是使用 GitHub Actions。

{% blockquote GitHub 帮助 https://help.github.com/cn/actions %}
GitHub 操作 帮助您在您存储代码的同一位置自动执行软件开发工作流程，并协作处理拉取请求和议题。 您可以写入个别任务，称为操作，并结合它们创建一个自定义的工作流程。 工作流程是您可以在仓库中创建的自定义自动化流程，用于在 GitHub 上构建、测试、封装、发行或部署任何代码项目。

通过 GitHub 操作 可直接在仓库中构建端到端持续集成 (CI) 和持续部署 (CD) 功能。
{% endblockquote %}

那么 GitHub Actions 怎么写呢？敲黑板啦！下面开始抄作业啦。

### 添加密钥

首先去 GitHub repo 设置里添加需要用到的密钥（Settings >> Secrets）。用什么对象存储就添加什么的密钥，用不上就没必要添加。（不用纠结下面跟我的图不一样）
![创建完示意图](https://image-1253491707.file.myqcloud.com/2020-3-24-13-11-32.png/webp)

#### COS 相关密钥

`COS_BUCKET`：COS 存储桶名称，包含后面的数字，如：`test-1234567`。
`COS_REGION`：COS 存储桶区域，如：`ap-chengdu`。
`COS_SECRET_ID`：能访问到那个存储桶的 secret id，可以去控制台生成。
`COS_SECRET_KEY`：secret id 对应的 secret key。

#### OSS 相关密钥

`OSS_BUCKET`：OSS 存储桶名字。
`OSS_ENDPOINT`：OSS 地域节点，如：`oss-cn-hongkong.aliyuncs.com`。
`OSS_KEY_ID`：OSS access key id，同样去控制台生成。
`OSS_KEY_SECRET`：OSS access key id 对应的密钥。**注意！密钥生成完只显示一次！**

#### telegram 相关密钥

telegram 相关的密钥不是必须的，发送部署通知到 TG 才需要这些密钥。

`TG_BOT_TOKEN`：telegram bot token，如：`123456789:QWERTYUIOPASDFGHJKLZXCVBNM`
`TG_CHAT_ID`：telegram chat id，可以为群组或个人的 id。

### 创建 Actions

点击 repo 主界面的 Actions 选项然后选择 Set up a workflow yourself。
![创建 actions](https://image-1253491707.file.myqcloud.com/2020-3-24-15-3-51.png/webp)
然后从下面的选项卡中选择对应的内容复制粘贴进去就好。
**如果你的分支不是 `master` 需要去 `branches` 切换成自己的分支！**
**如果使用的是 `npm` 而不是 `yarn` 需要自行替换相关位置的命令！**

{% tabs 选项卡, 1 %}
<!-- tab 部署博客到 COS -->
``` yaml
name: Deployment

# Controls when the action will run. Triggers the workflow on push or pull request
# events but only for the hexo branch (change to your hexo blog branch)
on:
  push:
    branches: [master]

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  hexo-deployment:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - name: Checkout source
        uses: actions/checkout@v1
        with:
          submodules: true

      - name: Setup python
        uses: actions/setup-python@v1
        with:
          python-version: '3.6'

      - name: Change post files modified time
        run: |
          find source/_posts -name '*.md' | while read file
          do touch -d "$(git log -1 --format="@%ct" "$file")" "$file"
          done

      - name: Install dependencies
        run: |
          yarn global add hexo-cli
          yarn install

      - name: Generate site
        run: |
          export PATH="$PATH:`yarn global bin`"
          hexo clean
          hexo g

      - name: Setup coscmd
        env:
          SECRET_ID: ${{ secrets.COS_SECRET_ID }}
          SECRET_KEY: ${{ secrets.COS_SECRET_SECERT }}
          BUCKET: ${{ secrets.COS_BUCKET }}
          REGION: ${{ secrets.COS_REGION }}
        run: |
          pip install coscmd
          coscmd config -a $SECRET_ID -s $SECRET_KEY -b $BUCKET -r $REGION -m 30

      - name: Deploy to cos
        run: coscmd upload -rs --delete -f public/ /
```
<!-- endtab -->
<!-- tab 部署博客到 OSS-->
``` yaml
name: Deployment

# Controls when the action will run. Triggers the workflow on push or pull request
# events but only for the hexo branch (change to your hexo blog branch)
on:
  push:
    branches: [master]

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  hexo-deployment:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - name: Checkout source
        uses: actions/checkout@v1
        with:
          submodules: true

      - name: Change post files modified time
        run: |
          find source/_posts -name '*.md' | while read file
          do touch -d "$(git log -1 --format="@%ct" "$file")" "$file"
          done

      - name: Install dependencies
        run: |
          yarn global add hexo-cli
          yarn install

      - name: Generate site
        run: |
          export PATH="$PATH:`yarn global bin`"
          hexo clean
          hexo g

      - name: Setup oss util
        uses: manyuanrong/setup-ossutil@v1.0
        with:
          endpoint: '${{ secrets.OSS_ENDPOINT }}'
          access-key-id: '${{ secrets.OSS_KEY_ID }}'
          access-key-secret: '${{ secrets.OSS_KEY_SECRET }}'

      - name: Clean bucket
        run: ossutil rm oss://${{ secrets.OSS_BUCKET }}/ -rf

      - name: dDeploy to oss
        run: ossutil cp public/ oss://${{ secrets.OSS_BUCKET }}/ -rf
```
<!-- endtab -->
<!-- tab 缓存依赖加速构建 -->
默认每次都会下载依赖使构建速度变慢，GitHub 官方提供了 cache actions 来加快构建速度。以 `yarn` 为例，只需要将下面的代码加到 `checkout source` 步骤后面即可。如果使用的是 npm 话查看[官方的例子](https://github.com/actions/cache/blob/master/examples.md#node---npm)。**注意缩进！**

```yaml deployment.yaml https://github.com/actions/cache/blob/master/examples.md#node---yarn examples.md#node - yarn
- name: Get yarn cache directory path
  id: yarn-cache-dir-path
  run: echo "::set-output name=dir::$(yarn cache dir)"

- uses: actions/cache@v1
  id: yarn-cache # use this to check for `cache-hit` (`steps.yarn-cache.outputs.cache-hit != 'true'`)
  with:
    path: ${{ steps.yarn-cache-dir-path.outputs.dir }}
    key: ${{ runner.os }}-yarn-${{ hashFiles('**/yarn.lock') }}
    restore-keys: |
      ${{ runner.os }}-yarn-
```

腾讯云 coscmd 是使用 pip 安装的，如果是使用的 COS 的话还可以缓存 pip 模块提高速度。添加的位置跟上面一样。

```yaml deployment.yaml https://github.com/actions/cache/blob/master/examples.md#python---pip examples.md#python - pip
- name: Get pip cache
  id: pip-cache
  run: |
    python -c "from pip._internal.locations import USER_CACHE_DIR; print('::set-output name=dir::' + USER_CACHE_DIR)"

- name: Get coscmd's requirements
  run: wget https://raw.githubusercontent.com/tencentyun/coscmd/master/requirements.txt

- name: Pip cache
  uses: actions/cache@v1
  with:
    path: ${{ steps.pip-cache.outputs.dir }}
    key: ${{ runner.os }}-pip-${{ hashFiles('**/requirements.txt') }}
    restore-keys: |
      ${{ runner.os }}-pip-
```

<!-- endtab -->
<!-- tab 发送部署通知到 TG -->
想让部署通知发送到 TG 只需要在 actions 最后面添加以下内容即可（前提是添加过相关密钥）。**注意缩进！**

```yaml
- name: Notification
  if: cancelled() == false
  uses: xinthink/action-telegram@v1.1
  with:
    botToken: ${{ secrets.TG_BOT_TOKEN }}
    chatId: ${{ secrets.TG_CHAT_ID }}
    jobStatus: ${{ job.status }}
    skipSuccess: false
```

![通知预览图](https://image-1253491707.file.myqcloud.com/2020-3-24-15-41-12.png/webp)
<!-- endtab -->
{% endtabs %}

### 保存 Actions

复制粘贴修改完成后检查一下，检查无误就可以提交啦。点击 Start commit 然后点击 Commit new file 就好啦。
![保存 Actions](https://image-1253491707.file.myqcloud.com/2020-3-24-15-45-50/webp)
然后 Actions 就会自动运行一次。如果能运行就没有问题啦。

最后贴一个博客的测速镇楼，整体速度还是不错的。（香港 OSS，未使用 CDN）
![博客测速图](https://image-1253491707.file.myqcloud.com/2020-3-24-17-3-23.png/webp)
