---
layout: post
title: Jekyll on Github 收工
---

捣鼓好了Jekyll，距离我第一次对这个系统感兴趣和尝试搭建，已经过去了17个月。网上能看到不少Jekyll的教程，基本按步骤来就能设置好。一顿实操下来，发现最重要的还是要理解Jekyll的工作方式，知道摆弄哪些文档会产生哪些变化就行。后期的样式修改、插件应用等『增强型』用法，慢慢来再说了。

网上大部分教程将 Jekyll 跟 Github 混着写，还有打头就让注册域名的，这些教程在搭好之后完整地看没什么问题，对完全没概念的人来说估计有些迷糊（猜的）。

所以下面是我根据自己理解的搭建顺序写的步骤。

## 本地安装Jekyll

Jekyll 提供将文档根据一定规则拼装成静态站点的功能，因为能够在本地安装并运行，所以先搭好环境。

-   [JekyllCN | 安装](http://www.jekyllcn.com/docs/installation/)：Jekyll官网中文站，这一页主要讲Jekyll环境建立，页面给的流很完善，跟着来不会弄错。
-   [Jekyll](http://www.jekyllrb.com)：官网，有的内容看原文更容易理解些，用作参考。

## Jekyll目录结构

有文档、有样式、有布局，一个站点就这么成了。重点是要以Jekyll的方式来成。

-   [JekyllCN | 目录结构](http://jekyllcn.com/docs/structure/)：理解Jekyll的目录结构，明白哪些生产页面、哪些定义布局、文档存放位置，以及一些针对Jekyll的优化。大部分教程在这里会紧接着讲解 `_config.yml` 的用法，但实际需要定义的内容，是根据使用者需求来的，基本参数可以直观理解，后期加入扩展等操作时，可以看到更有针对性的修改。
-   [使用Github Pages建独立博客](http://beiyuu.com/github-pages/)：BeiYuu 的这篇教程关于 Jekyll 基本结构的部分写得比较清楚。

## 理解结构

理解结构，通过实例来学习最方便了，所以推荐直接找个（喜欢的）模板查看代码并调试。

Github 上的模板都可以下载 `.zip` 压缩包到本地，解压后 `jekyll server` 一下直接生成模板demo，此时可以随意更改内容，以观察Jekyll的结构。例外是修改 `_config.yml` 需要关掉并重启 `jekyll server` 才会产生变化。

-   [Lanyon](https://github.com/poole/lanyon)：我正在使用的模板，个人认为页面结构简单文档规范，比较适合学习。
-   [jekyll Themes](http://jekyllthemes.org/)：大量Jekyll模板，找到自己的喜欢的不难。
-   其它教程里推荐的个人站点，多看没错。

## Github

本地调试好，接下来就是找在线数据托管了。Github 和 Jekyll 基本被看作一体，所以推荐使用 Github。



...to be continued