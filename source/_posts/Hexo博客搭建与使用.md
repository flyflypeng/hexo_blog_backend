---
title: Hexo博客搭建与使用
date: 2015-07-13 20:18
categories: 技术
tags:
- Hexo
- 博客
---

### 目录
>* [Hexo 简介](#1)
>* [Hexo 安装](#2)
>* [Hexo 博客网站搭建](#3)
>* [网站配置](#4)
>* [Hexo命令使用](#5)
>* [文章编辑及发布](#6)
>* [主题优化](#7)


## Hexo简介 <span id="1"></span>
Hexo是一款快速、简洁和高效的一款静态博客框架，它出自于台湾的一名大学生Tommy Chen之手。这款博客框架程序是基于Node.js开发的，它编译上百篇的博文也只仅仅需要几秒就可以了，非常快速。并且，Hexo也是一款非常受程序猿喜欢的博客搭建框架，它使用Markdown解析文章，同时你还可以将你的网站非常简单快速地部署到github page上。Hexo还具有非常丰富的插件系统，用户可以基于这些插件来做一些网站的扩展。

如果你觉得搭建的过程还是有些繁琐，但是当你一旦搭建完成之后，你用Hexo来发表你用Markdown写的文章，你就会觉得这是一件很轻松很舒服的事情。有关详情你可以进入[Hexo][Hexo_url]的中文网站查看相关文档。

<!--more-->

## Hexo安装 <span id="2"></span>

### 系统环境
由于本人是一个程序猿，平时使用较多的是Linux操作系统，所以本文是基于Ubuntu 14.04 64位系统环境搭建的Hexo博客。如果你使用的是windows系统，那么请移步到文章末尾有关windows平台的安装教程的链接，[点此跳转](#reference_link)。

### 安装
在安装Hexo之前，我们首先需要安装下面的必备软件。

#### Node.js
安装[Node.js][Node_js_url]，你可以点击上面的链接进入Node.js的官网，下载下源码自己手动编译安装，具体的步骤可以参见其文档。在安装完成Node.js之后，它自带了包管理器npm。最后一步，我们还需要确定一下我们的Node.js是否安装好了，我们可以通过以下的命令来测试Node.js是否安装成功。

![node_js][node_version]

如果出现了如上图所示的版本信息结果之后，则说明Node.js已经安装成功了。

### Git
 [Git][Git_url] 是由Linux之父Linus开发的一款分布式的代码管理工具，它能够很方便地管理本地的代码，而且还可以将本地代码提交到远程的github网站上进行代码托管。

Github是全球最大的代码托管网站，同时也是全球最大的开源软件托管网站，在这里我们可以寻找到许许多多非常优秀的开源软件。除了代码托管的功能之外，github网站还推出了github pages的功能。Github pages它又可以分为两种类型:
* 项目页面站点
* 个人/组织页面站点

有关Github pages的相关内容，请详见[Github pages][Github_pages_url]的主页。

如果你想将你的博客网站托管到github pages上，你就需要在你的Hexo配置文件中配置好有关设置。所以Git在Hexo博客搭建中，它最重要的作用就是将我们用Hexo生成的本地静态网页部署到远程的github上对应的网站仓库中。

有关Git的安装配置，请详见 [Git][Git_url] 的官方网站，参考相关文档完成安装。

### Hexo
完成Node.js和Git的安装之后，下面就可以通过Node.js自带的包管理器安装Hexo了。
安装的命令如下:
```
npm install -g hexo-cli
```
在安装完成之后，我们可以通过以下命令来查看安装的hexo的版本:
```
hexo version
```
![hexo][hexo_version]

## Hexo博客网站搭建 <span id="3"></span>

完成了Hexo安装之后，我们需要在自己的本地磁盘上新建一个文件夹，用来存放Hexo博客网站的相关文件。执行完以下的命令之后，在相应的目录下就会自动生成Hexo博客网站所需的相关文件。
```
$ hexo init <folder>
$ cd <folder>
$ npm install
```
以后，有关hexo的相关命令均在此<folder>目录下进行。

### 目录结构
新建完成之后，指定的文件夹的目录如下所示:
![hexo_directory][hexo_dir_url]

#### _config.yml
网站的配置文件，你可以在该文件中配置绝大多数的参数。

### package.json
应用程序的信息。EJS, Stylus 和 Markdown renderer 已默认安装，您可以自由移除。
![package_json][package_json_url]

### scaffolds
模板文件夹。当您新建文章时，Hexo会根据scaffolds来建立文件。

### scripts
脚本文件夹。脚本是扩展 Hexo 最简易的方式，在此文件夹内的 JavaScript 文件会被自动执行。

### source
资源文件夹是存放用户资源的地方。除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。

### themes
主题文件夹。Hexo会根据主题来生成静态页面。有关Hexo的不同主题，详见文章最后的参考链接。

### 测试

在完成上述的安装配置之后，一个默认的Hexo博客网站就搭建好了，我们可以使用以下命令，启动本地服务
```
hexo server
```
在启动完成本地服务之后，我们可以根据命令行窗口中的提示在浏览器中输入 http://0.0.0.0:4000 就能在浏览器中查看到Hexo默认生成的静态网站的页面。以后，我们在自己编写好博文之后，我们也可以通过这种方式首先在本地先预览下静态网页的效果，如果没有问题，则再通过hexo d 命令部署到github网站上。

## 网站配置 <span id="4"></span>
下面，我就以自己Hexo博客网站中的_config.yml配置文件来解释各个配置项的意义。
```
# Hexo Configuration
## Docs: http://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site  #站点信息
title: Woshijpf's Blog  #标题
subtitle:  #副标题
description:  #站点描述，给搜索引擎看的
author: woshijpf  #作者信息
language: zh-CN   #语言
timezone:         #时区

# URL  #链接格式
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://www.woshijpf.info  #网址
root: /  #网站根目录
permalink: :year/:month/:day/:title/  #文章的链接格式
permalink_defaults:  ##默认的文章链接格式

# Directory  #目录格式
source_dir: source  #源文件目录
public_dir: public  #生成的静态网页文件的目录
tag_dir: tags  #标签目录
archive_dir: archives   #存档目录
category_dir: categories   #分类目录
code_dir: downloads/code   #代码目录
i18n_dir: :lang
skip_render:

# Writing  #写作
new_post_name: :title.md # File name of new posts  #新文章的表题
default_layout: post  #默认的模板，包括post、photo、page和draft（文章、照片、页面和草稿）
titlecase: false # Transform title into titlecase  #是否将标题转换成大写
external_link: true # Open external links in new tab  #是否在新标签页中打开外部连接
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
highlight:  #语法高亮
  enable: true  #是否启用
  line_number: true  #是否启用行号
  tab_replace:

# Category & Tag   #分类和标签
default_category: uncategorized  #默认分类
category_map:
tag_map:

# Archives 默认值为2，这里都修改为1，相应页面就只会列出标题，而非全文
## 2: Enable pagination  #开启分页
## 1: Disable pagination #禁用分页
## 0: Fully Disable      #全部禁用
archive: 1

# Date / Time format  #日期/时间时间格式
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD   #日期默认格式：年-月-日
time_format: HH:mm:ss     #时间格式：小时-分钟-秒

# Pagination  #分页
# Set per_page to 0 to disable pagination  #如果把per_page设置为0，则表示禁用分页
per_page: 5  #每页显示的文章数
pagination_dir: page

# Extensions  #扩展插件
## Plugins: http://hexo.io/plugins/
exclude_generator: 

## Themes: http://hexo.io/themes/  #主题
theme: yilia

# Deployment  #部署，即将网站部署到github上的配置
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: git  #表示使用git工具进行部署
  repository: git@github.com:woshijpf/woshijpf.github.io.git  #网站部署到的仓库ssh地地址
  branch: master  #部署到仓库的分支

plugins:   #插件，例如生成rss订阅和站点地图
- hexo-generator-feed
- hexo-generator-sitemap
- hexo-generator-baidu-sitemap

baidusitemap:
    path: baidusitemap.xml

```

## Hexo命令使用 <span id="5"></span>
### init
```
$ hexo init [folder]
```
新建一个网站。如果没有设置folder，则默认在当前文件夹建立网站。

### new
```
$ hexo new [layout] <title>
```
新建一篇文章。如果没有设置 layout 的话，默认使用 _config.yml 中的 default_layout 参数代替。如果标题包含空格的话，请使用引号括起来。

### generate
```
$ hexo generate
```
生成静态网页文件。

|     选项    |         描述        |
|------------|--------------------|
|-d,- -deploy| 文件生成后立即部署网站 |
|-w,- - watch| 监视文件变动          |

### publish
```
# hexo publish [layout] <filename>
```
发辫草稿箱目录_drafts下的草稿。

### server
```
$ hexo server
```
启动服务器。

|     选项    |         描述        |
|------------|--------------------|
|-p,- -port  | 文件生成后立即部署网站 |
|-s,- -static| 只使用静态网页        |
|-l,- -log   | 启动日志记录，或覆盖记录格式|

### deploy
```
$ hexo deploy
```
部署网站。

|     选项    |         描述        |
|------------|--------------------|
|-g,- -generate | 部署网站前，需要预先生成网站的静态网页 |

### render
```
$ hexo render <file> ...
```
渲染文件

|     选项    |         描述        |
|------------|--------------------|
|-o,- -output | 设置输出路径|

### migrate
```
$ hexo migrate <type>
```
从其他系统迁移内容。

### clean
```
$ hexo clean
```
清除缓存文件（db.json）和已经生成的静态文件（public）。

### list
```
$ hexo list <type>
```
列出网站资料。

### version
```
$ hexo version
```
显示hexo版本。

### 常用命令简写
```
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```

### 插件安装
Hexo采用Node.js中的npm包管理器安装插件。
```
npm install <plugin-name> --save
npm update #升级
npm uninstall <plugin-name> #卸载
```
其中<plugin-name>是插件名称。

### 主题安装
```
git clone <repository> themes/<theme-name>
```
其中，<repository>是主题theme-name在github上的仓库地址，<theme-name>是存放在Hexo博客文件夹下theme文件夹中的主题目录名称。


## 文章编辑及发布 <span id="6"></span>
### 新建文章
```
$ hexo new "标题"
```
当在命令行窗口中执行上述命令之后，在source/_posts目录下会生成一个文件名"标题.md"的文件。

### Front Matter
Front-matter 是文件最上方以 --- 分隔的区域，用于指定个别文件的变量，举例来说：
```
title: Hello World
date: 2013/7/13 20:46:25
---
```
以下是预先定义的参数，您可以在模板中使用这些参数值并加以利用。

|     参数      |        描述      |      默认值    |
|--------------|------------------|----------- ---|
|layout        |布局              |               |
|title         |标题              |               |
|date          |建立日期           |文件建立日期     |
|updated       |更新日期           |文件更新日期     |
|commets       |开启文章评论功能    |true           |
|tags          |标签              |               |
|categories    |分类              |               |
|permalink     |覆盖文章网址       |                |

### 发布
本文以发布到github上的github pages为例。
编辑全局配置文件 _config.yml 中的 deploy 部分
```
# Deployment  #部署，即将网站部署到github上的配置
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: git  #表示使用git工具进行部署
  repository: git@github.com:woshijpf/woshijpf.github.io.git  #网站部署到的仓库ssh地地址
  branch: master  #部署到仓库的分支
```
完成上述的网站全局配置文件中的网站部署设置之后，下面只需要执行静态网页生成和网站部署的命令，如下所示:
```
$ hexo generate -d
```

## 主题优化  <span id="7"></span>
Hexo博客支持个性化的主题定制，用户可以根据自己的喜好来选择自己喜欢的模板，如果想自己动手设计并制作主题也是可以的。
下面是Hexo的[主题列表][Hexo_themes_url2]，你可以在这上面找到自己喜欢的主题。

### 主题下载
你可以通过git工具从github上下载下主题。
```
git clone <repository> themes/<theme-name>
```
当然，你也可以在github相关主题的网页上直接下载压缩文件，然后将主题解压到指定文件夹内即可。

完成主题下载后，只需要在网站全局配置文件_config.yml文件中theme那一行中将你所下载下来的主题名称填写到“theme: "后面即可（此处要注意你所填写的主题名称要与你在themes目录中的主题文件夹的文件夹名相同）。

### 主题目录
我们下载下来的主题目录的结构如下图所示:
![theme_dir][theme_dir_url]

### 主题配置文件
每一个主题都有一个叫_config.yml的配置文件，在这里一定要注意区分一下，在hexo中我们需要配置两个都叫做_config.yml的配置文件。
* 一个在hexo博客网站的所在文件夹的根目录中，它是配置整个博客网站。
* 一个在themes目录下不同主题文件夹中,它是配置不同的主题样式。

下面就以我所使用的主题[yilia][yilia_url]的配置文件为例。
```
# Header
menu:  #主题主界面左边显示的菜单项设置
  主页: /
  所有文章: /archives
  随笔: /categories/随笔
  技术: /categories/技术
  设计: /categories/设计

# SubNav
subnav:  //子导航项
  github: "https://github.com/woshijpf"
  weibo: "http://weibo.com/2434965031/profile?topnav=1&wvr=6"
  rss: "/atom.xml"
  zhihu: "http://www.zhihu.com/people/jiang-peng-fei-24"
  douban: "http://www.douban.com/people/56692971/"
  mail: "#"
  #facebook: "#"
  #google: "#"
  #twitter: "#"
  #linkedin: "#"

rss: /atom.xml  #rss订阅

# Content
excerpt_link: more  #“Read more”需要显示的文字
fancybox: true #fancybox 看图效果
mathjax: true  #公式

# Miscellaneous
google_analytics: UA-63075912-1  #Google Analytics ID
favicon: /favicon.ico  #网站图标

#你的头像url
avatar: "http://7xj51c.com1.z0.glb.clouddn.com/labixiaoxin.jpg"
#是否开启分享
share: true
#是否开启多说评论，填写你在多说申请的项目名称 duoshuo: duoshuo-key
#若使用disqus，请在博客config文件中填写disqus_shortname，并关闭多说评论
duoshuo: true
#是否开启云标签
tagcloud: true

#是否开启友情链接
#不开启——
#friends: false
#开启——
friends:
  酷壳: http://coolshell.cn/
  IBM developerWorks: https://www.ibm.com/developerworks/cn/
  matrix67: http://www.matrix67.com/
  灵犀志趣: http://www.lingcc.com/
  西南交通大学: http://www.swjtu.edu.cn/
  浙江大学: http://www.zju.edu.cn/

#是否开启“关于我”。
#不开启——
#aboutme: false
#开启——
aboutme: 或者是明天，或者是梦里。我重新念起我最爱的诗。我独自一人，感动了所有的风和日丽。
```


## Hexo搭建参考网站链接 <span id="reference_link"></span>
1.[Hexo中文官方网站][Hexo_url]
2.[使用Hexo搭建博客][Hexo_url_1]
3.[Hexo主题][Hexo_themes_url]
4.[有那些好看的hexo主题？][Hexo_themes_url1]
5.[Hexo主题列表][Hexo_themes_url2]
6.[hexo你的博客][Hexo_your_blog_url]
7.[Hexo使用多说教程][Hexo_duoshuo_url]
8.[在github上用hexo搭建博客][github_hexo_url]



[Hexo_url]:https://hexo.io/zh-cn/
[Node_js_url]:https://nodejs.org/
[Git_url]:http://git-scm.com/
[Github_pages_url]:https://pages.github.com/
[Hexo_url_1]:blog.lmintlcx.com/post/blog-with-hexo.html
[Hexo_themes_url]:https://hexo.io/themes/
[Hexo_themes_url1]:http://www.zhihu.com/question/24422335/answer/46357100
[Hexo_themes_url2]:https://github.com/hexojs/hexo/wiki/Themes
[yilia_url]:https://github.com/litten/hexo-theme-yilia
[Hexo_your_blog_url]:http://ibruce.info/2013/11/22/hexo-your-blog/
[Hexo_duoshuo_url]:http://dev.duoshuo.com/threads/541d3b2b40b5abcd2e4df0e9
[github_hexo_url]:http://pleasureswx123.github.io/2014/08/29/%E5%9C%A8github%E4%B8%8A%E7%94%A8hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/


[node_version]:http://7xj51c.com1.z0.glb.clouddn.com/node_version.png
[hexo_version]:http://7xj51c.com1.z0.glb.clouddn.com/hexo_version.png
[hexo_dir_url]:http://7xj51c.com1.z0.glb.clouddn.com/hexo目录.png
[package_json_url]:http://7xj51c.com1.z0.glb.clouddn.com/package_jason.png
[theme_dir_url]:http://7xj51c.com1.z0.glb.clouddn.com/theme_dir.png