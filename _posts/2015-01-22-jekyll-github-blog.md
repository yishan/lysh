---
layout: post
title: Jekyll on Github 收工
comments: true
---

捣鼓好了 Jekyll，新的写作站点也同时建好了，想起我第一次对这个东西感兴趣和尝试搭建，已经过去了17个月，结果今年才搞定。

网上能看到不少 Jekyll 的教程，基本按步骤来就能设置好。一顿实操下来，发现最重要的还是要理解 Jekyll 的工作方式，知道摆弄哪些文档会产生哪些变化就行。后期的样式修改、插件应用等『增强型』用法，慢慢来再说了。

大部分教程将 Jekyll 跟 Github 混着写，还有打头就让注册域名的，这些教程在搭好之后完整地看没什么问题，对完全没概念的人来说估计有些迷糊（猜的）。

所以下面是我根据自己文科生思维所理解的搭建顺序写的步骤，以及可能需要用到的参考资料。

## 本地安装 Jekyll

Jekyll 是一个静态网站生成器，提供将文档根据一定规则拼装成静态站点的功能。

Jekyll 可以运行在本地，所以首先推荐先搭好工作环境。

-   [JekyllCN | 安装](http://www.jekyllcn.com/docs/installation/)：Jekyll 官网中文版，这一页主要讲 Jekyll 环境建立，页面给的流很完善，跟着来不会弄错。
-   [Jekyll](http://www.jekyllrb.com)：官网，原先的 Jekyll wiki 的内容现在都迁移到这个站点上的。有的内容看原文更容易理解些，用作参考。

## 理解 Jekyll

要知道 Jekyll，推荐直接找个（喜欢的）模板查看目录结构就行。

-   [JekyllCN | 目录结构](http://jekyllcn.com/docs/structure/)：理解Jekyll的目录结构，明白哪些生产页面、哪些定义布局、文档存放位置，以及一些针对Jekyll的优化。大部分教程在这里会紧接着讲解 `_config.yml` 的用法，但实际需要定义的内容，是根据使用者需求来的，基本参数可以直观理解，后期加入扩展等操作时，可以看到更有针对性的修改。
-   [使用Github Pages建独立博客](http://beiyuu.com/github-pages/)：这篇教程关于 Jekyll 基本结构的部分写得比较清楚。

Github 上的模板都可以下载 `.zip` 压缩包到本地，将文档解压后放到自行定义的工作路径可以观察 Jekyll 的结构。

-   [Lanyon](https://github.com/poole/lanyon)：我正在使用的模板，页面结构简单文档规范，适合学习。
-   [jekyll Themes](http://jekyllthemes.org/)：大量Jekyll模板，找到自己的喜欢的不难。
-   其它教程里推荐的个人站点，多看没错。

## Markdown

通常这些模板都是带简介和测试文章的，于是能看到站点文章都存放在 `_posts` 文件夹里。Jekyll 上写作的文档基本以 `.md` （或者 `.markdown`)作为后缀，那么需要了解下什么是 Markdown。

-   [Markdown写作浅谈](http://www.yangzhiping.com/tech/r-markdown-knitr.html)：这篇文章对 Markdown 有比较全面的讲解。
-   [DaringFireball: Markdown](http://daringfireball.net/projects/markdown/)：Markdown 语法创造者 John Gruber 站点关于 Markdown 的详细说明，可作为索引使用。

这两个连接能了解到如何创建、修改以及使用 Markdown 文档，如此 Blog 的第一篇文章可以尝试着在 `_posts` 文件夹内创建并撰写。

## 本地测试

到这里，一个使用了你挑选的模板，有第一篇文章的 Blog 基本拥有所有部件的。但还没成型，现在让 Jekyll 出场。

以下仅以 Windows 系统做说明。打开控制台，进入文件夹，使用 Jekyll 命令生成站点

    jekyll server
    
有的教程会带上参数 `jekyll server --watch` 为了监控更新，但我没加参数时 Jekyll 也会自动重新生成修改后的页面，当然带上参数应该是个好习惯。
    
运行后控制台会出现如下字样

       Server address: http://127.0.0.1:4000/
    Server running...  press ctrl-c to stop.
    
打开浏览器，访问以上地址，就可以看到运行在本地，已经经过 Jekyll 生成的站点了。

此时本地文件夹里会更新 `_site_` 文件夹，这里是经过 Jekyll 处理后的出产物，一个编译好的完整站点。

## Github 

本地调试好，如果希望在线访问，就要轮到 Github 出场了。

Github 是基于 Git 这一版本控制系统的代码分享社区，也是 Blog 计划存放的地方。

-   [Git - Book](http://git-scm.com/book/zh/v1/)：这是 [Pro Git](http://book.douban.com/subject/3420144/)这本书的在线中译本，前两章可以简单了解下 Git 和工作原理。
-   [如何高效利用 Github](http://www.yangzhiping.com/tech/github.html)：这里有一些基础和进阶用法。

许多教程写作于13年或者更早时期，会在这里教学如何使用在命令行中使用 `git` 命令更新文档那个和站点，但如果只是要更新 Blog，安装官方的 Github for Windows 软件会是更好的选择。

-   [Github for Windows](https://windows.github.com/) 安装后自动在机子内安装 Git ，可以开始使用相关指令，并同时包含图形用户界面和命令行的操作方式。一般撰写者只需要看懂下面这个形象的控制器就行。

## Repository 项目库

安装后可以通过软件将本地文档上传到 Github 了。

把 Blog 架设到 Github 有两个方法，只是顺序的不同，最终结果都可以是在本地和 Github 上实现数据同步。下面是我使用的方法。

在 Github 注册帐号，新建一个项目（Repository），并命名为 `username.github.io` ，如下图

![Create a repository](\assets\2015_01\createrepo.png =600x)

1.  打开项目页面，在页面右侧能看到 `Clone in Desktop` 字样，点击后自动启用 Github for Windows 将项目同步到本地，并在本地生成与项目名称同名的文件夹。
2.  将先前下载并解压、修改的模板文件夹里的所有文档，整个儿放到项目同名文件夹中。

OK，你已经拥有一个在线的 Github 空间（以项目库的形式出现），一个本地的、结构完整的 Blog，只缺让文档同步、存储到在线空间这一步了。

打开 Github for Windows

![Github for Window Main](\assets\2015_01\githubforwinmain.png =600x)

1.  有修改但未提交同步的文件，因为之前同步在本地的项目文件夹基本是空的（应该只包含一个 `README.md` 文档），所以需要同步的文件会比图里显示的多一些
2.  此次修改的说明
3.  项目库列表

单击右上角 `Sync` 按钮，等待提交数据，完成后无论本地还是在线的项目库文档都是最新的。

## 事就这么成了

现在访问 username.github.io 就能看到你的站点。

后续写作的操作大概就是本地撰写文档，然后提交到 Github 的过程。

## 定制化 和 其它

### 自定义域名
-   [Setting up a custom domain with GitHub Pages](https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages/)：官方帮助文档
-   [Github的自定义域名设置](http://blog.cssforest.org/2014/11/06/Github%E7%9A%84%E8%87%AA%E5%AE%9A%E4%B9%89%E5%9F%9F%E5%90%8D%E8%AE%BE%E7%BD%AE.html)：基础设置方法
-   [浅谈github页面域名绑定](http://yanping.me/cn/blog/2011/12/04/github-pages-domain/)：延展使用方法

### 评论
-   [How I Created a Beautiful and Minimal Blog Using Jekyll, Github Pages, and poole](http://joshualande.com/jekyll-github-pages-poole/)：添加 Disqus 评论模块。这篇教程还包括了如何添加 Google Analytics 和 推特 的更新等设置，可以一并了解。
-   [在Github Pages中集成多说评论插件](http://code4pub.github.io/tech/2014/05/04/integrate-with-duoshuo-comment/)：添加[多说](http://duoshuo.com/)评论模块

### JekyllBootstrap

[JekyllBootstrap](http://jekyllbootstrap.com/) 是个快速建站的工具，提供若干模板，帮助新来者不需埋首学习 Jekyll 所采用的 Liquid 模板语言（相关中文文档数量较少）。通过 JekyllBootstrap 建立一个可用的站点只需要 3 分钟（官方宣传），但因为我在两年前使用这个方法没能太好的理解 Jekyll 和 Github，所以把它放在最后提了。只是对在 Github 弄个单纯写字的地方有兴趣的话，可以不看之前的内容，直接到 JekyllBootstrap 体验一下。 

### Markdown 写作工具

Markdown 的写作工具不少，在线的如 [Cmd Markdown](http://www.zybuluo.com/mdeditor)、[StackEdit](https://stackedit.io/) 都不错。知乎上有相关的问题讨论 —— [用 Markdown 写作用什么文本编辑器？](http://www.zhihu.com/question/19637157)，推荐了不少，可以挑选使用。

为了方便跟站点一起调试摆弄，我用的是 [Brackets](http://brackets.io)，加上插件后的效果大概如图所示。Brackets 的特性不少，最新版还能直接从 PSD 抓到图层的数据并给出 CSS 提示，简直了。

![Brackets with Markdown Plugin](\assets\2015_01\bracketswithmdplugin.png =600x)

### 参考

其它内容较全看得也挺清楚的教程

-   [用Jekyll构建静态网站](http://yanping.me/cn/blog/2011/12/15/building-static-sites-with-jekyll/)
-   [Build A Blog With Jekyll And GitHub Pages](http://www.smashingmagazine.com/2014/08/01/build-blog-jekyll-github-pages/)