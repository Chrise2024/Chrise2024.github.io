---
title: Python-环境配置
description: 环境搭起来
tags: [Programing,Python,Python-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-01-25 19:25 +0800
nav:
  prev:
    path: posts/python/python-prelude
    title: Python-序
  next:
    path: posts/python/python-firstclass
    title: Python-入门第一课
--- 

`Python`的开发环境包括解释器和编辑器两部分。`Python`必须要有一个解释器才行，同时还需要一个文本编辑器来写源代码。可以选择IDE打包完成，也可以选择解释器+文本编辑器的组合。

## IDE

目前能进行`Python`开发的IDE有很多，官方发行版会自带`IDLE`作为IDE。这里只介绍，`Visual Studio`和`JetBrains PyCharm`

### Visual Studio

`Microsoft`家的超级IDE，功能齐全。对于学习来说，免费的Community版本完全够用（[安装器](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=Community&channel=Release&version=VS2022&source=VSLandingPage&cid=2030&passive=false)）

下载完成后启动安装器，工作负荷选择“Python开发”，可以选择是否安装`Python`，然后开始安装即可。

### PyCharm

`PyCharm`是`JetBrains`出品的`Python`，功能强大，有免费的Community版本。（[安装包](https://www.jetbrains.com/pycharm/download/other.html)）

安装完成之后开箱即用。

## 解释器

这里只建议使用官方发行的解释器

[下载](https://www.python.org/ftp/python/3.13.1/python-3.13.1-amd64.exe)，在安装过程中建议勾选`Add to Path`省去手动配置`Path`的麻烦。

## 文本编辑器

任意文本编辑器都可以用于`Python`编程，记事本都行。这里只讲较为常用的`VSCode`（Visual Studio Code）。`VSCode`是`Microsoft`家的文本编辑器，完全免费，还有一个社区的孪生兄弟`VSCodium`，是`VSCode`的开源版本。

1. [下载](https://code.visualstudio.com/docs/?dv=win64user)
2. 安装`Python`插件（全名`Python`，是`Microsoft`官方的）。

## 写在最后

至此，你已经有了一个能开发`Python`的环境了。下一节，将正式开始`Python`之旅。
