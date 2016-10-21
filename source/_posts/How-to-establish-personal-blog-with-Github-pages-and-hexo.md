title: Github Pages和hexo建立个人博客教程
date: 2015-07-28 16:00:46
keywords: Github pages,hexo,blog
bibliography: bibliography.bib
tags:
---
#前言
一直以来都有亲自搭建一个个人博客记录下自己生活学习中点滴的想法。每次想花时间着手去做这些工作的时候，都以迷失中Google的汪洋大海之中告终。久而久之这个“宏大”的想法也就被抛诸脑后了。时间不知不觉就到2015年7月27日，学校里的老师同学差不多都去度假了。老板走之前给我留下了一堆任务，比如NS-3的安装使用。于是又像往常一样。我在网上搜集着各路牛人写的技术博客，各种借鉴之后，总算是把NS-3调理顺了。跟以前一样，觉得这时候把这些安装啊配置啊搞得清楚，指不定过了几个月这些get的技能就又丢了。一想到之前的人生就是不断地在记住遗忘之中度过，我觉得这次绝对不可以再拖了，必须着手搞一搞自己的私人博客了，既可以锻炼自己web方面的技能，又可以记录下自己的学习生活。实在妙极啊！刚好这两周老板不在，利用这个时间给我充个电~

#更新 2016-10-20
好久不写博客了，怎么现在重新拾起，却发现，诸如npm, hexo 之类的命令不管用了呢？
莫非需要重来一遍。。。oh no...

ruby的版本号太老了 需要更新
rvm 是专门管理 ruby 的各版本的，而 Homebrew 只是用来管理 OSX 确实的 packages 的，两者不要混淆。对于本博客，rvm 不是太需要

#技术方案选择
Google了一下，网上推荐度最高的就是个人博客方案就是: Github pages + hexo + 多说 + 七牛 + Doddady + DNSPro了，所以其他技术方案，也有各自的亮点，但是本博客就不予考虑了。


#原理
知其然还要知其所以然，在这个知识爆炸的年代，我们越来越少有时间去搞明白知识的来龙去脉，尤其是在IT领域，备受推崇的所谓的快速学习现学现卖的能力。我发现，网上大多数关于个人博客搭建的帖子，都是直接告诉你一步步该怎么做，至于总体的原理却无人涉及，所以经常会有些网友在比着葫芦画票的过程中迷失了，遇到一些错误，因为他们不明白为什么我们需要这样的配置或者命令。这种总体原理上认知的缺失是我在网上有这么多安装教程存在的情况下，依然想自己去总结写作的动机。


#环境准备
本博客主要是针对Mac环境下的个人博客搭建，如果是Windows平台的，可以移步至cnfeat前辈的大作，目前知乎上推荐度最高的，个人博客搭建教程：[如何搭建一个独立博客——简明Github Pages与Hexo教程](http://cnfeat.com/2014/05/10/2014-05-11-how-to-build-a-blog/)。另外看到一些用Windows系统的程序员在网上调侃自嘲自己是屌丝穷码畜，戏称用Mac的为码帅。虽然我用的也是Mac,但我也是广大的屌丝阵营的一员，在努力着逆袭走上人生巅峰迎娶白富美。不过使用过Windows，Linux(Ubuntu/Mint)以及Mac OS, 个人感觉，还是Mac OS用户体验最好。Windows我就直接懒得吐槽了，Linux虽然也不错，但是用户界面还有稳定性还是不如Mac OS。有人会说，Mac OS没有原声的包管理工具(apt-get)之类的，但是有第三方包管理神奇Homebrew啊。更重要的是，Mac有我见过的最好的的触摸板支持以及手势操作。刚从Linux转移使用Mac OS，一开始还不太习惯。后来用多了发现Mac OS真的好用，我已经一年多没再用过鼠标了， 当然要说Mac的缺点，也有，一个字：贵。

## 安装git, node.js, npm
网上相关的安装教程多如牛毛，我个人推荐使用Homebrew安装，具体流程参见：
[How to Install Xcode, Homebrew, Git, RVM, Ruby & Rails on Mac OS X (From Snow Leopard to Yosemite)](http://www.moncefbelyamani.com/how-to-install-xcode-homebrew-git-rvm-ruby-on-mac/)
[如何在Mac Yosemite中安装node.js以及NPM](http://rennesong.com/2015/07/29/How-to-install-node-js-and-npm-on-Mac/)

## 安装hexo以及必须的node.js模块
首先在电脑中自己觉得合适的位置，创建一个专门用来写博客的文件夹，并且切换至这个文件夹
```shell
    mkdir Personal_Blog && cd Personal_Blog
```
接着使用Node.js的包管理器npm安装hexo模板引擎，注意选项g代表了gloablly
```shell
    npm install hexo -g
```
待hexo安装完毕后，用hexo初始化这个文件夹(我创建的文件夹叫做Personal_Blog)。(和git的用法好像)
```shell
    hexo init
```
这时候Terminal里会出现提示：
```shell
INFO  Copying data to ~/Documents/Personal_Blog
INFO  You are almost done! Don't forget to run 'npm install' before you start blogging with Hexo!
```
而且，这个文件夹会多出一些文件(一开始是空的)：
```shell
total 24
drwxr-xr-x   8 qsong  staff   272B Jul 30 14:30 .
drwx------+ 36 qsong  staff   1.2K Jul 30 14:30 ..
-rw-r--r--   1 qsong  staff    65B Jul 30 14:30 .gitignore
-rw-r--r--   1 qsong  staff   1.4K Jul 30 14:30 _config.yml
-rw-r--r--   1 qsong  staff   442B Jul 30 14:30 package.json
drwxr-xr-x   5 qsong  staff   170B Jul 30 14:30 scaffolds
drwxr-xr-x   3 qsong  staff   102B Jul 30 14:30 source
drwxr-xr-x   3 qsong  staff   102B Jul 30 14:30 themes
```
根据系统的提示，我们需要安装node.js需要的模块，只需要执行：
```shell
    npm install 
```
为了让hexo把网站代码推送到githubs上，我们还需要下面这个module
```shell
    npm install hexo-deployer-git --save
```
至此，我们可以说个人博客就这么搭建好了，简单吧。

##hexo配置文件
网站搭建完成后，就可以根据自己爱好来对Hexo生成的网站进行设置了，对整站的设置，只要修改项目目录的_config.yml就可以了，这是我的设置，可供参考。
```shell
# Hexo Configuration
## Docs: http://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site
title: Rennesong's blog  #替换为需要的博客名称
subtitle: Blog my life from now on!
description: Personal blog site, to note technique and life related articles
author: rennsong
language: zh-CN #拿中文写作
timezone:

# URL
url: http://rennesong.com
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:

# Directory
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:

# Writing
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link: true # Open external links in new tab
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
highlight:
  enable: true
  line_number: false
  auto_detect: true
  tab_replace:

# Category & Tag
default_category: uncategorized
category_map:
tag_map:

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10
pagination_dir: page

duoshuo_shortname: rennesong
# Extensions
## Plugins: http://hexo.io/plugins/
## Themes: http://hexo.io/themes/

plugins:
- hexo-generator-sitemap
- hexo-generator-feed
theme: landscape

sitemap:
    path: sitemap.xml
baidusitemap:
    path: baidusitemap.xml
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
    type: git   #现在都用git了，如果github则无效
    repo: git@github.com:hansomesong/hansomesong.github.io.git
    branch: master
```

# 社会化评论支持
Hexo默认支持的第三方社会化评论插件是Disqus, 国内多使用***多说***。鉴于本博客由汉语书写，主要针对中国同胞了，所以决定采用多说。具体安装过程参见zipperary大作[hexo系列教程:(四) hexo博客的优化技巧](http://zipperary.com/2013/05/30/hexo-guide-4/)。其实很简单，概括起来就是：第一要去多说网站上注册一个账户，创建一个域名。第二就是把相应的Javascript代码粘贴到自己使用模板的相应文件里(具体代码文件因模板而异)。比如zipperary上面的帖子就只是针对他使用的主题light的，但是他依然给出了含有Hexo默认主题的相关Javascript的代码的帖子。

# 生成站点地图
```shell
npm install hexo-generator-sitemap --save
```
然后编辑_config.xml文件(注意不是theme的_config.xml文件)

```shell
plugins:
- hexo-generator-sitemap
sitemap:
	path: sitemap.xml
```

注意YAML格式的配置文件要去冒号后面有个空格

# Hexo 常见命令
这里有必要提下Hexo常用的几个命令：
hexo generate (hexo g) 生成静态文件，会在当前目录下生成一个新的叫做public的文件夹
hexo server (hexo s) 启动本地web服务，用于博客的预览
hexo deploy (hexo d) 部署播客到远端（比如github, heroku等平台）

# Hexo 升级之后的坑
本文成文的时候，Hexo还是2.X版本，目前Hexo已经从2.x系列升级为3.x系列。导致一了一些模板以及插件不能正常使用的情况。比如我在执行 hexo g 的时候，报错如下：
```
INFO  Start processing
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Template render error: (unknown path) [Line 7, Column 23]
  Error: Unable to call `the return value of (posts["first"])["updated"]["toISOString"]`, which is undefined or falsey
    at Object.exports.prettifyError (/Users/qsong/Documents/song/personal_blog/node_modules/nunjucks/src/lib.js:34:15)
    at /Users/qsong/Documents/song/personal_blog/node_modules/nunjucks/src/environment.js:486:31
    at new_cls.root [as rootRenderFunc] (eval at _compile (/Users/qsong/Documents/song/personal_blog/node_modules/nunjucks/src/environment.js:565:24), <anonymous>:161:3)
    at new_cls.render (/Users/qsong/Documents/song/personal_blog/node_modules/nunjucks/src/environment.js:479:15)
    at Hexo.module.exports (/Users/qsong/Documents/song/personal_blog/node_modules/hexo-generator-feed/lib/generator.js:28:22)
    at Hexo.tryCatcher (/Users/qsong/Documents/song/personal_blog/node_modules/bluebird/js/release/util.js:16:23)
    at Hexo.<anonymous> (/Users/qsong/Documents/song/personal_blog/node_modules/bluebird/js/release/method.js:15:34)
    at /Users/qsong/Documents/song/personal_blog/node_modules/hexo/lib/hexo/index.js:337:24
    at tryCatcher (/Users/qsong/Documents/song/personal_blog/node_modules/bluebird/js/release/util.js:16:23)
    at MappingPromiseArray._promiseFulfilled (/Users/qsong/Documents/song/personal_blog/node_modules/bluebird/js/release/map.js:61:38)
    at MappingPromiseArray.PromiseArray._iterate (/Users/qsong/Documents/song/personal_blog/node_modules/bluebird/js/release/promise_array.js:113:31)
    at MappingPromiseArray.init (/Users/qsong/Documents/song/personal_blog/node_modules/bluebird/js/release/promise_array.js:77:10)
    at MappingPromiseArray._asyncInit (/Users/qsong/Documents/song/personal_blog/node_modules/bluebird/js/release/map.js:30:10)
    at Async._drainQueue (/Users/qsong/Documents/song/personal_blog/node_modules/bluebird/js/release/async.js:143:12)
    at Async._drainQueues (/Users/qsong/Documents/song/personal_blog/node_modules/bluebird/js/release/async.js:148:10)
    at Immediate.Async.drainQueues (/Users/qsong/Documents/song/personal_blog/node_modules/bluebird/js/release/async.js:17:14)
    at runCallback (timers.js:637:20)
    at tryOnImmediate (timers.js:610:5)
    at processImmediate [as _immediateCallback] (timers.js:582:5)
FATAL (unknown path) [Line 7, Column 23]
  Error: Unable to call `the return value of (posts["first"])["updated"]["toISOString"]`, which is undefined or falsey
Template render error: (unknown path) [Line 7, Column 23]
  Error: Unable to call `the return value of (posts["first"])["updated"]["toISOString"]`, which is undefined or falsey
    at Object.exports.prettifyError (/Users/qsong/Documents/song/personal_blog/node_modules/nunjucks/src/lib.js:34:15)
    at /Users/qsong/Documents/song/personal_blog/node_modules/nunjucks/src/environment.js:486:31
    at new_cls.root [as rootRenderFunc] (eval at _compile (/Users/qsong/Documents/song/personal_blog/node_modules/nunjucks/src/environment.js:565:24), <anonymous>:161:3)
    at new_cls.render (/Users/qsong/Documents/song/personal_blog/node_modules/nunjucks/src/environment.js:479:15)
    at Hexo.module.exports (/Users/qsong/Documents/song/personal_blog/node_modules/hexo-generator-feed/lib/generator.js:28:22)
    at Hexo.tryCatcher (/Users/qsong/Documents/song/personal_blog/node_modules/bluebird/js/release/util.js:16:23)
    at Hexo.<anonymous> (/Users/qsong/Documents/song/personal_blog/node_modules/bluebird/js/release/method.js:15:34)
    at /Users/qsong/Documents/song/personal_blog/node_modules/hexo/lib/hexo/index.js:337:24
    at tryCatcher (/Users/qsong/Documents/song/personal_blog/node_modules/bluebird/js/release/util.js:16:23)
    at MappingPromiseArray._promiseFulfilled (/Users/qsong/Documents/song/personal_blog/node_modules/bluebird/js/release/map.js:61:38)
    at MappingPromiseArray.PromiseArray._iterate (/Users/qsong/Documents/song/personal_blog/node_modules/bluebird/js/release/promise_array.js:113:31)
    at MappingPromiseArray.init (/Users/qsong/Documents/song/personal_blog/node_modules/bluebird/js/release/promise_array.js:77:10)
    at MappingPromiseArray._asyncInit (/Users/qsong/Documents/song/personal_blog/node_modules/bluebird/js/release/map.js:30:10)
    at Async._drainQueue (/Users/qsong/Documents/song/personal_blog/node_modules/bluebird/js/release/async.js:143:12)
    at Async._drainQueues (/Users/qsong/Documents/song/personal_blog/node_modules/bluebird/js/release/async.js:148:10)
    at Immediate.Async.drainQueues (/Users/qsong/Documents/song/personal_blog/node_modules/bluebird/js/release/async.js:17:14)
    at runCallback (timers.js:637:20)
    at tryOnImmediate (timers.js:610:5)
    at processImmediate [as _immediateCallback] (timers.js:582:5)
```
原因就是插件 hexo-generate-feed 还有 hexo-generate-sitemap 和 hexo3 不兼容！

# 参考文献
[为Hexo博客添加目录](http://kuangqi.me/tricks/enable-table-of-contents-on-hexo/), 截止目前，本博客关于生成目录的方法参考了此贴

