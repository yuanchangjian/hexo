---
title: Hexo搭建博客系列：（一）搭建属于自己的博客网站
date: 2020-05-11 20:18:48
categories: Hexo搭建博客系列
tags: 
    - Hexo
---


# 前言
本教程是对Hexo+NexT博客部署到GitHub Pages的总结，你可以参照这个教程搭建出基本能用的博客网站，若你有自己的想法，可以自己参考相关文档进行个性化配置。针对后续我自己博客的个性化配置，我会在这一系列教程的其他教程中一一列举出来。

<!-- more -->

# Hexo建站
[Hexo](https://hexo.io/zh-cn/)是一个基于[Node.js](https://nodejs.org/en/)的静态博客框架，它使用Markdown(或其他渲染引擎)解析文章，快速且高效。

## 安装前提
需要先安装下列应用程序：
* Node.js(最新版本，我的版本是V.12.13.0)
* Git

## 安装Hexo
```
$ npm install hexo-cli -g
```

## 建站
```
$ hexo init <folder>
$ cd <folder>
$ npm install
```
新建完成后，文件夹目录如下所示：
```
├── _config.yml
├── package.json
├── scaffolds
├── source
└── themes
```
### _config.yml
网站的[配置](https://hexo.io/zh-cn/docs/configuration)信息。

### package.json
应用程序的信息。

### scaffolds
[模版](https://hexo.io/zh-cn/docs/writing)文件夹。当您新建文章时，Hexo 会根据scaffold来建立文件。

### source
资源文件夹是存放用户资源的地方。除```_posts```文件夹之外，开头命名为```_```(下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到```public```文件夹，而其他文件会被拷贝过去。

### themes
[主题](https://hexo.io/zh-cn/docs/themes)文件夹。Hexo会根据主题来生成静态页面。

# NexT主题
[NexT](http://theme-next.iissnan.com)是Hexo主题中最受欢迎的一款主题（star最多），风格简洁，确实很好看。HexT的GitHub代码仓库经常变动，主要是下列3个仓库:
* [2014-2017版本(当前最新V5.1.4)](https://github.com/iissnan/hexo-theme-next)
* [2018-2019(当前最新V7.8.0)](https://github.com/theme-next/hexo-theme-next)
* [2020(```当前最新版本v8.0.0-rc.2，使用版本```)](https://github.com/next-theme/hexo-theme-next)

在 Hexo 中有两份主要的配置文件，其名称都是 _config.yml。 其中，一份位于站点根目录下，主要包含 Hexo 本身的配置；另一份位于主题目录下，这份配置由主题作者提供，主要用于配置主题相关的选项。
为了描述方便，在以下说明中，将前者称为```站点配置文件```， 后者称为```主题配置文件```

## 安装NexT
Hexo主题安装非常简单，只需要将主题文件拷贝至站点目录```themes```目录下，将主题目录命名为```next```。

## 启动主题
打开```站点配置文件```， 找到```theme```字段，并将其值更改为```next```。

## 验证主题
```
$ cd <folder>
$ npm run server
```
使用浏览器访问```http://localhost:4000```，检查站点是否正确运行。![image-20200515102636697](https://yuanchangjian.github.io/cloudImage/images/20200515102636697.png)

现在，你已经成功安装并启用了 NexT 主题。下一步我们将对主题进行个性化配置。



## 主题配置
主题生效后，我们便开始配置主题的基本设置。

### 设置站点标题
编辑```站点配置文件```， 将```title```设置成你的站点标题。
```
title: Ycj's Blog
```

### 设置站点描述

编辑```站点配置文件```， 设置```description```字段为你的站点描述。
```
description: 我是要成为海贼王的男人
```

### 设置作者昵称
编辑```站点配置文件```， 设置```author```为你的昵称。
```
author: Ycj
```

### 设置语言
编辑```站点配置文件```， 将```language```设置成你所需要的语言。
```
language: zh-CN
```

### 设置外观
Scheme 的切换通过更改```主题配置文件```，搜索```scheme```关键字。目前NexT支持四种外观，分别是：
* Muse
* Mist
* Pisces
* Gemini

我比较喜欢```Mist```这种风格，你可以自己都尝试一下。
```
# Schemes
#scheme: Muse
scheme: Mist
#scheme: Pisces
#scheme: Gemini
```

### 设置菜单
NexT默认菜单项如下所示，我当前使用了```home(首页)```、```categories(分类)```、```archives(归档)```、```tags(标签)```和```搜索```（后续提到）。
```
menu:
  home: / || fa fa-home
  categories: /categories/ || fa fa-th
  archives: /archives/ || fa fa-archive
  tags: /tags/ || fa fa-tags
  # about: /about/ || fa fa-user
  # schedule: /schedule/ || fa fa-calendar
  # sitemap: /sitemap.xml || fa fa-sitemap
  # commonweal: /404/ || fa fa-heartbeat
```

#### 添加「分类」页面
```
$ cd <folder>
$ hexo new page categories
```

将新建的source/categories目录下的index.md增加type说明：

```
---
title: categories
type: categories
date: 2020-05-12 10:55:35
---
```

#### 添加「标签」页面

```
$ cd <folder>
$ hexo new page tags
```

将新建的```source/tags```目录下的```index.md```增加```type```说明：

```
---
title: tags
type: tags
date: 2020-05-12 10:56:30
---
```

#### 文章中添加标签和分类

你可以在```文章```中的```Front-matter```中增加分类和标签，如：

```
---
title: Hello World
categories:
- Diary
tags:
- PS3
- Games
---
```

{% note danger %} 

**分类方法的分歧**

如果您有过使用 WordPress 的经验，就很容易误解 Hexo 的分类方式。WordPress 支持对一篇文章设置多个分类，而且这些分类可以是同级的，也可以是父子分类。但是 Hexo 不支持指定多个同级分类。下面的指定方法：

```
categories:
  - Diary
  - Life
```

会使分类`Life`成为`Diary`的子分类，而不是并列分类。因此，有必要为您的文章选择尽可能准确的分类。

如果你需要为文章添加多个分类，可以尝试以下 list 中的方法。

```
categories:
- [Diary, PlayStation]
- [Diary, Games]
- [Life]
```

此时这篇文章同时包括三个分类： `PlayStation` 和 `Games` 分别都是父分类 `Diary` 的子分类，同时 `Life` 是一个没有子分类的分类。

{% endnote %}

### 设置侧栏

可以通过修改```主题配置文件```中的```sidebar```字段来控制侧栏的行为。设置侧栏的位置，修改```sidebar.position```的值，支持的选项有：
* left - 靠左放置
* right - 靠右放置

```
sidebar:
  #position: left
  position: right
```

### 设置头像
编辑```主题配置文件```， 修改```avatar```下的```url```字段，url可以是站内或完整的互联网URI:
* 完整的互联网URI
* 站点内的地址，放置在主题```source/images/```目录下

```
avatar:
  url: /images/avatar.gif
```

### 设置RSS
安装```hexo-generator-feed```插件，它是hexo博客专门生成RSS xml文件的插件。
```
npm install hexo-generator-feed --save
```
编辑```站点配置文件```，在文件末尾添加下列配置：
```
# rss配置
feed:
  type: atom
  path: atom.xml
  limit: 20
```

{% note warning %} RSS自动生成按钮在NexT V7.6.0版本移除，V7.6.0以下的NexT版本的博客网站普遍都有一个RSS按钮，不用担心，我们可以通过下面提到的设置社交链接展示出来。 {% endnote %}

### 设置社交链接
编辑```主题配置文件```， 在```social```中添加社交链接：
```
social:
  GitHub: https://github.com/yuanchangjian || fab fa-github
  RSS: /atom.xml || fa fa-rss
  # 等等
```

### 设置友情链接
编辑```主题配置文件```，在```links```下添加你的友情链接地址：
```
links:
  CoolShell: https://coolshell.cn/
  Eric: 'https://github.com/lizhaoting'
```

编辑```主题配置文件```，在```links_settings```下修改为下列配置。链接项布局默认为```block```，我觉得和我这个风格不搭，我改为```inline```。
```
links_settings:
  icon: fa fa-link
  title: 友情链接
  # Available values: block | inline
  layout: inline
```

### 添加腾讯公益404页面
腾讯公益404页面，寻找丢失儿童，让大家一起关注此项公益事业！
使用方法，新建```404.html```页面，放到主题的```source```目录下，内容如下：
```
<!DOCTYPE HTML>
<html>
<head>
  <meta http-equiv="content-type" content="text/html;charset=utf-8;"/>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <meta name="robots" content="all" />
  <meta name="robots" content="index,follow"/>
  <link rel="stylesheet" type="text/css" href="https://qzone.qq.com/gy/404/style/404style.css">
</head>
<body>
  <script type="text/plain" src="http://www.qq.com/404/search_children.js"
          charset="utf-8" homePageUrl="/"
          homePageName="回到我的主页">
  </script>
  <script src="https://qzone.qq.com/gy/404/data.js" charset="utf-8"></script>
  <script src="https://qzone.qq.com/gy/404/page.js" charset="utf-8"></script>
</body>
</html>
```

### 设置站点建立时间
这个时间将在站点的底部显示，例如 © 2020 - 2022。 编辑```主题配置文件```，修改```since```字段：
```
footer:
  # Specify the date when the site was setup. If not defined, current year will be used.
  since: 2020
```

现在，你已经完成了主题的基本配置，页面展示如下所示：

![image-20200515110048107](https://yuanchangjian.github.io/cloudImage/images/20200515110050.png)

****



## 集成常用的第三方服务

### 搜索服务

相较官网提供几种[搜索服务](http://theme-next.iissnan.com/third-party-services.html#analytics-system)，我这里使用的是```Local Search```（这个配置最简单，站内查文章够用，当然想百度统计也比较常用，喜欢的可以自行添加），提供一个页面快速文章的功能。

1. 安装 `hexo-generator-searchdb`，在站点的根目录下执行以下命令：

   ```
   $ npm install hexo-generator-searchdb --save
   ```

2. 编辑 **站点配置文件**，新增以下内容到任意位置：

   ```
   search:
     path: search.xml
     field: post
     format: html
     limit: 10000
   ```

3. 编辑 **主题配置文件**，启用本地搜索功能：

   ```
   # Local search
   local_search:
     enable: true
   ```

重启应用，效果如下：![image-20200515110258588](https://yuanchangjian.github.io/cloudImage/images/20200515110300.png)



### 数据统计与分析

我这里使用了[不蒜子统计](http://theme-next.iissnan.com/third-party-services.html#analytics-busuanzi)+LeanCloud实现站内的数据统计，这里提及一下，不蒜子现在版本对于首页多个文章无法统计阅读量，所以我i将不蒜子是用来统计网站访问量和用户量，LeanCloud用来统计文章的阅读量。

* 不蒜子统计

  编辑主题配置文件，找到```busuanzi_count```字段，修改为如下所示:

  ```
  busuanzi_count:
    enable: true
    total_visitors: true
    total_visitors_icon: fa fa-user
    total_views: true
    total_views_icon: fa fa-eye
    post_views: false
    post_views_icon: fa fa-eye
  ```

效果图如下（统计数不对是因为开发环境如此，部署会正确展示）：![image-20200515111951296](https://yuanchangjian.github.io/cloudImage/images/20200515111953.png)

* [LeanCloud](https://www.leancloud.cn/)

1. 创建应用

   应用名随意填写，不满意后续可更改。

   ![image-20200515113112829](https://yuanchangjian.github.io/cloudImage/images/20200515113114.png)

2. 新建Class

   为了保证我们前面对NexT主题的修改兼容，此处的新建Class名字必须为```Counter```。

   ![image-20200515113916349](https://yuanchangjian.github.io/cloudImage/images/20200515113917.png)

   ![image-20200515113948683](https://yuanchangjian.github.io/cloudImage/images/20200515113950.png)

   ![image-20200515114552823](https://yuanchangjian.github.io/cloudImage/images/20200515115930.png)



3. 配置LeanCloud

   获取```app_id```和```app_key```

   ![image-20200515115918389](https://yuanchangjian.github.io/cloudImage/images/20200515115919.png)

   编辑主题配置文件，找到`leancloud_visitors`字段，将你的`app_id`和`app_key`填上，并修改`security`为`false`，修改为如下所示:

   ```
   leancloud_visitors:
     enable: ture
     app_id: xxx # <your app id>
     app_key: xxx # <your app key>
     server_url: # <your server url>
     security: false
   ```



# 部署至GitHub Pages

部署参考[官网部署教程](https://hexo.io/zh-cn/docs/github-pages)，这里我列出来是因为我想通过`<GitHub 用户名>.github.io`访问博客，但我发现`GitHub Pages`的部署分支只能为`master`，所以我修改了`.travis.yaml`文件，使我们在其他分支中（如：`hexo`）开发，而通过Travis CI工具部署至`master`分支，从而保证`<你的 GitHub 用户名>.github.io`能正常访问。

如果你不需要`<GitHub 用户名>.github.io`如此访问，可以接受`<GitHub 用户名>.github.io/xxx`访问就无需更改`.travis.yaml`文件，可以跳过本小节教程，直接参考官网部署教程。

1. 新建一个 repository。如果你希望你的站点能通过 `<你的 GitHub 用户名>.github.io` 域名访问，你的 repository 应该直接命名为 `<你的 GitHub 用户名>.github.io`。

2. 将你的 Hexo 站点文件夹推送到 repository 中。默认情况下不应该 `public` 目录将不会被推送到 repository 中，你应该检查 `.gitignore` 文件中是否包含 `public` 一行，如果没有请加上。

3. 将 [Travis CI](https://github.com/marketplace/travis-ci) 添加到你的 GitHub 账户中。

4. 前往 GitHub 的 [Applications settings](https://github.com/settings/installations)，配置 Travis CI 权限，使其能够访问你的 repository。

5. 你应该会被重定向到 Travis CI 的页面。如果没有，请 [手动前往](https://travis-ci.com/)。

6. 在浏览器新建一个标签页，前往 GitHub [新建 Personal Access Token](https://github.com/settings/tokens)，只勾选 `repo` 的权限并生成一个新的 Token。Token 生成后请复制并保存好。

7. 回到 Travis CI，前往你的 repository 的设置页面，在 **Environment Variables** 下新建一个环境变量，**Name** 为 `GH_TOKEN`，**Value** 为刚才你在 GitHub 生成的 Token。确保 **DISPLAY VALUE IN BUILD LOG** 保持 **不被勾选** 避免你的 Token 泄漏。点击 **Add** 保存。

8. **在你的 Hexo 站点文件夹中新建一个 `.travis.yml` 文件：**

   ```
   sudo: false
   language: node_js
   node_js:
     - 10 # use nodejs v10 LTS
   cache: npm
   branches:
     only:
       - hexo # build master branch only (修改点：master -> hexo)
   script:
     - hexo generate # generate static files
   deploy:
     provider: pages
     skip-cleanup: true
     github-token: $GH_TOKEN
     keep-history: true
     on:
       branch: hexo # (修改点：master -> hexo)
     local-dir: public
     target-branch: master # (增加项：将hexo分支编译到master分支部署)
   ```

   

# 参考资料

* [Hexo官方网站](https://hexo.io/zh-cn/)

* [Hexo官方使用文档](http://theme-next.iissnan.com/)

* [为NexT主题添加文章阅读量统计功能](https://notes.doublemine.me/2015-10-21-%E4%B8%BANexT%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0%E6%96%87%E7%AB%A0%E9%98%85%E8%AF%BB%E9%87%8F%E7%BB%9F%E8%AE%A1%E5%8A%9F%E8%83%BD.html#%E9%85%8D%E7%BD%AELeanCloud)

* [Travis CI官方说明文档](https://docs.travis-ci.com/)