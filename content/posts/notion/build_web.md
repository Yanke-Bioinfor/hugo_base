---
title: "创站记录"
date: PublishDate

# weight: 1

# aliases: ["/first"]

tags: ["new"]
author: "亱梦言柯"

# author: ["Me", "You"] # multiple authors

showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: ""
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/hugo_base/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

# 初见·Notion

从19年开始使用Notion，后来因为疫情缘故，组织了一些抗疫相关页面，Notion官方赠送了免费的team plan，到现在已经三年了。

![Untitled](../554d6cd2_Untitled.png)

# 缘起·Blog

一直喜欢Notion的编辑风格，却一直没能妥当的使用起来。

最近想到了搭建一个博客，在众多博客类型里挑花了眼。是在服务器搭一个nginx服务器，或者利用wordpress快速起模板，再或者用hugo来做静态网站。

研究了半天，不管是自己搭框架做编辑器，还是写MarkDown，都不如Notion编辑器来的舒坦。所以重心就放在如果将Notion做成一个可以做个人主页的博客系统。

# Vercel + Notion

第一个主页性质的页面 [Curtis‘ Home Page](https://liyanke.notion.site]) 就诞生了，来自@**heyalber**设计的模板。

我有一个十年的域名biocat.top，遂希望能用自己的域名访问Notion的网址。经过一番调试，最终采用Vercel+notion的解决方案。

Vercel 是一个云原生的全球部署平台，专注于静态网站和单页面应用程序的托管和部署。它提供了一个简单易用的平台，可以让开发者轻松地将其应用程序部署到全球各地的服务器上。

<span style='color:gray'>Vercel 支持多种前端框架，包括 React、Vue、Angular、Next.js 等。它还提供了强大的功能，如自动缓存更新、预渲染、灰度发布等，以帮助开发者轻松管理和优化他们的应用程序。</span>

<span style='color:gray'>Vercel 还与现代开发工具和流行的版本控制系统集成，例如 GitHub、GitLab 和 Bitbucket。这使得团队能够使用他们熟悉的工具进行协作和部署。</span>

<span style='color:gray'>总的来说，Vercel 是一个强大的部署平台，它简化了应用程序的部署过程，并提供了优秀的性能和全球范围的可扩展性。无论是个人开发者还是大型团队，都可以从 Vercel 提供的功能和便利性中受益。</span>

当时我还不知道Notion Team Plan提供了免费的主页域名，可以通过自定义的域名访问设定的主页，也就是现在[liyanke.notion.site](http://liyanke.notion.site/)，同时也用Vercel搭建了一个网址[https://notion.biocat.top/](https://notion.biocat.top/)。

如你所见，这两个页面的内容是一致的。我期望利用Notion的Database做一个集成的博客系统。目前，分类也还没有确定，但是可以预期会包含，知识库、生活随笔、突发奇想、追的番/剧、饮食记录。

关于Vercel的操作部分，我已经忘记了，这就是没有随时记录的坏处，要不要重新整理下呢。还不知道。接下来就是Hugo + Github Pages + Actions自动化更新Hugo博客了

# Hugo + Github

对于内容创作来说，高质量的内容才是重中之重，无可奈何，自己还是写不出来高质量作品来，慢慢练习，增强书面和口语表达能力。尽管工具可能是其次的，多尝试下也能为以后坚持做铺垫。

## 配置hugo

Hugo可以在多平台部署，我拿的就是之前的centos服务器上运行了，Hugo创建静态网站还算简单。

### 下载Hugo编译好的版本，甚至都不需要go环境。


```shell
wget https://github.com/gohugoio/hugo/releases/download/v0.121.1/hugo_0.121.1_Linux-64bit.tar.gz
tar -zxvf hugo_0.121.1_Linux-64bit.tar.gz
mv hugo /usr/bin/
```

### 创建Hugo网站


```shell
hugo new site liyanke
```

### 配置Hugo模板

Hugo官网支持多种模板，[Themes](https://themes.gohugo.io/)，我选择了Papermod，看起来比较清爽，重点是他支持直接展示个人详情页，非常适合当主页。


```shell
cd liyanke
git init
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
git submodule update --init --recursive #为了配置后面的Github Action
git submodule update --remote #可以同步主题仓库的最新修改。
```

安装好Hugo和模板之后可以参考模板的要求修改Hugo目录下的hugo.yaml，这里初始应是hugo.toml。两个文件模式都可以。这里使用是papermod提供yaml模板。


```yaml
baseURL: "https://yanke-bioinfor.github.io" #这里修改为你自己网站
title: Curtis' Home Page
paginate: 5
theme: PaperMod

enableRobotsTXT: true
buildDrafts: false
buildFuture: false
buildExpired: false

googleAnalytics: UA-123-45

minify:
  disableXML: true
  minifyOutput: true

params:
  env: production # to enable google analytics, opengraph, twitter-cards and schema.
  title: Curtis' Home Page
  description: "Curtis' Home Page"
  keywords: [Blog, Portfolio, PaperMod]
  author: Me
  # author: ["Me", "You"] # multiple authors
  images: ["<link or path of image for opengraph, twitter-cards>"]
  DateFormat: "January 2, 2006"
  defaultTheme: auto # dark, light
  disableThemeToggle: false

  ShowReadingTime: true
  ShowShareButtons: true
  ShowPostNavLinks: true
  ShowBreadCrumbs: true
  ShowCodeCopyButtons: false
  ShowWordCount: true
  ShowRssButtonInSectionTermList: true
  UseHugoToc: true
  disableSpecial1stPost: false
  disableScrollToTop: false
  comments: false
  hidemeta: false
  hideSummary: false
  showtoc: false
  tocopen: false

  assets:
    # disableHLJS: true # to disable highlight.js
    disableFingerprinting: true
    favicon: "<link / abs url>"
    favicon16x16: "<link / abs url>"
    favicon32x32: "<link / abs url>"
    apple_touch_icon: "<link / abs url>"
    safari_pinned_tab: "<link / abs url>"

  label:
    text: "Home"
    icon: /apple-touch-icon.png
    iconHeight: 35

  # profile-mode
  profileMode:
    enabled: ture # needs to be explicitly set
    title: "Curtis' Home Page"
    subtitle: "Work As A Bioinformatics Engineer. \n\r You can find me at [Notion](https://liyanke.notion.site)"
    imageUrl: "https://avatars.githubusercontent.com/u/37400776"
    imageWidth: 120
    imageHeight: 120
    imageTitle: my image
    buttons:
      - name: Posts
        url: posts
      - name: Tags
        url: tags

  # home-info mode
  homeInfoParams:
    Title: "Hi there \U0001F44B"
    Content: Welcome to my blog

  socialIcons:
    - name: twitter
      url: "https://twitter.com/cloudstarfall"
    - name: stackoverflow
      url: "https://stackoverflow.com"
    - name: github
      url: "https://github.com/"

  analytics:
    google:
      SiteVerificationTag: "XYZabc"
    bing:
      SiteVerificationTag: "XYZabc"
    yandex:
      SiteVerificationTag: "XYZabc"

  cover:
    hidden: true # hide everywhere but not in structured data
    hiddenInList: true # hide on list pages and home
    hiddenInSingle: true # hide on single page

  editPost:
    URL: "https://github.com/<path_to_repo>/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link

  # for search
  # https://fusejs.io/api/options.html
  fuseOpts:
    isCaseSensitive: false
    shouldSort: true
    location: 0
    distance: 1000
    threshold: 0.4
    minMatchCharLength: 0
    keys: ["title", "permalink", "summary", "content"]
menu:
  main:
    - identifier: categories
      name: categories
      url: /categories/
      weight: 10
    - identifier: tags
      name: tags
      url: /tags/
      weight: 20
    - identifier: Notion
      name: Notion
      url: https://liyanke.notion.site
      weight: 30
# Read: https://github.com/adityatelange/hugo-PaperMod/wiki/FAQs#using-hugos-syntax-highlighter-chroma
pygmentsUseClasses: true
markup:
  highlight:
    noClasses: false
    # anchorLineNos: true
    # codeFences: true
    # guessSyntax: true
    # lineNos: true
    # style: monokai
```

对于新发表的md文件也有对应的模板，叫做front matter。

我这里的md模板如下：


```markdown
---
title: "Hello World"
date: now

# weight: 1

# aliases: ["/first"]

tags: ["new"]
author: "亱梦言柯"

# author: ["Me", "You"] # multiple authors

showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "描述文字"
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/hugo_base/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---
```

### 文件结构

Hugo可以将md文件编译成Html文件，需要你配置相应的目录结构。

[//]: # (column_list is not supported)

	[//]: # (column is not supported)

		![Untitled](../8b20176b_Untitled.png)

		<br/>

	[//]: # (column is not supported)

		默认的文件结构应该是content/posts

		content里面的每一个文件夹里都应该配置一个_index.md作为菜单的标记。

		
```yaml
---
title: "content/posts"
hidemeta: true
---
```

		<br/>

## 配置Github

在项目根目录下执行Hugo，就会在当前目录生成一个public文件夹，包含一整套静态网站的东西，这时候就可以通过创建git仓库然后配置pages来生成一个由github托管的免费的静态网站了。

但本文重点是利用Actions配置自动化更新的Hugo网站，最终实现本地配置git push就自动更新静态网站。思考一下，github部署的顺序是什么，不然容易脑袋大。

### 创建个人主页git仓库

个人主页仓库要求比较特殊，必须是<username>.github.io，这个库的名字也是github page初始的域名。像这里我只能是我的用户名+.github.io，忽视大小写。创建之后不用管。等创建一个hugo的仓库，维护我们md文件和创建action功能之后，这个库会自动更新的。

![Untitled](../9c0bcbd1_Untitled.png)

### 创建个人Token

在github设置里，不是仓库设置里，找到developer settings,添加一个repo和workflow的token。这个token只出现一次，并且有时效性，及时记录下来。

![Untitled](../6b96cfcb_Untitled.png)

### 创建hugo仓库

首先配置一下刚刚创建好的token,在hugo仓库的settings里。

![Untitled](../513c5960_Untitled.png)

接下来在hugo的仓库目录下创建一个.github/workflows文件夹，里面放入执行action的yml文件，可以放很多个，github action功能非常强大，他可以定时或者根据条件触发任务。这些任务可以是git，python，go等等程序。这里我们创建一个可以自动部署hugo的action。


```yaml
name: deploy

on:
    push:
    workflow_dispatch:
    schedule:
        # runs every 5 minutes
        - cron: '*/5 * * * *'

jobs:
    build:
        runs-on: ubuntu-latest
        steps:
            - name: Checkout
              uses: actions/checkout@v2
              with:
                  submodules: true
                  fetch-depth: 0

            - name: Setup Hugo
              uses: peaceiris/actions-hugo@v2
              with:
                  hugo-version: "latest"

            - name: Build Web
              run: hugo

            - name: Deploy Web
              uses: peaceiris/actions-gh-pages@v3
              with:
                  PERSONAL_TOKEN: ${{ secrets.PERSONAL_TOKEN }}
                  EXTERNAL_REPOSITORY: Yanke-Bioinfor/yanke-bioinfor.github.io
                  PUBLISH_BRANCH: master
                  PUBLISH_DIR: ./public
                  commit_message: ${{ github.event.head_commit.message }}
```

至此，hugo仓库的配置完毕，通过git push 到你github仓库就行了，他就可以自己开始部署到刚刚创建的[个人主页仓库](/fd788bde4e58433a9443eabb8b12e030#fb6bfd62481e404e938024e191183f19)里。

# 行缘·未来式

有了Github Action，理想情况只需要我在Notion编辑页面，就可以自动更新到Hugo，目前也看了一些作者实现了这个功能，但是我再重现就遇到了一些问题。

[Akkuman](https://www.cnblogs.com/Akkuman/p/15672566.html#%E5%9F%BA%E7%A1%80%E7%8E%AF%E5%A2%83)实现了我理想中的情况，但是我下载Notion的时候总是报错，目前还不知道什么原因，作者也停更了，估计维护不了了

[echo724](https://github.com/echo724/notion2md)开发了一个Notion转md的py库，但是他下载的页面总是不完全。

[Scarsu](/fd29727283534db6bff35658e3cc4f43)提到了自动化更新Notion，但是没有找到相关仓库。

[souvikinator](https://github.com/souvikinator/notion-to-md)开发了基于node.js的转换库，我不会用哈哈哈哈

目前来看这个想法暂时搁置一段时间，先把优质的内容做出来吧。

<br/>

再见👋🏻

<br/>

