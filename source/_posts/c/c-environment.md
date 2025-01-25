---
title: C-环境配置
description: 工欲善其事，必先利其器
tags: [Programing,C]
categories: Programing
math: true
lang: zh-CN
date: 2025-01-25 18:11 +0800
--- 

# C的学习与开发

`C`的开发环境包括编译器和编辑器两部分。作为编译型语言，那要进行开发必须要有一个编译器才行，同时还需要一个文本编辑器来写源代码。可以选择IDE一站到底，也可以选择编译器+文本编辑器的组合。

# 环境搭建

## IDE

目前能进行C开发的IDE有很多，这里只介绍`Dev-C++`、`Visual Studio`和`JetBrains CLion`

### Dev-C++

C语言老牌IDE，轻量简洁，自带MinGW编译环境，开箱即用，缺点则是过于简洁。

Dev-C++可以直接从Microsoft Store下载，也可以通过安装包手动下载（[安装包](https://sourceforge.net/projects/orwelldevcpp/files/Setup%20Releases/)）。

### Visual Studio

`Microsoft`家的超级IDE，功能齐全。对于学习来说，免费的Community版本完全够用（[安装器](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=Community&channel=Release&version=VS2022&source=VSLandingPage&cid=2030&passive=false)）

下载完成后启动安装器，工作负荷选择“使用C++的桌面开发”，然后开始安装即可。

> 实际上，`Visual Studio`并不包含`C`，你在里面写的所谓`C`只是用`C++`标准编译的`C`代码。同时加上`Visual Studio`提供的一些特性，你在`Visual Studio`写的“C代码”不一定能在其他真正的`C`环境下运行。

<Details>
<Summary>进阶配置</Summary>
安装VS含MSVC命令行工具，如需使用该命令行工具则需手动配置。

1. 找到你的VS安装目录，进入`VC/Tools/MSVC/<版本号>`文件夹。

![MSVCPath](https://c-environment.shigure.link/MSVCPath.jpg)

2. 进入`bin/Hostx64/x64`文件夹，复制完整文件夹路径（单击资源管理器上方地址栏的空白部分即可复制），然后追加到系统环境变量`Path`的末尾。

![HostPath](https://c-environment.shigure.link/CPHotsPath.jpg)

3. 回到`VC/Tools/MSVC/<版本号>`文件夹，复制`include`文件夹完整路径，添加新环境变量`INCLUDE`填入刚刚复制的路径。
4. 进入`lib/x64`文件夹，复制文件夹完整路径，添加新环境变量`LIB`填入刚刚复制的路径。
5. 找到`Windows Kits`安装目录，进入，选择对应Windows版本的文件夹，比如`Windows Kits/10`。

![WinKitPath](https://c-environment.shigure.link/WinKitPath.jpg)

6. 进入`Include\<版本号，建议选最新>`文件夹，依次复制`ucrt`、`um`、`winrt`三个文件夹的完整路径并追加到步骤3的`INCLUDE`环境变量后

![WKIncludeFolder](https://c-environment.shigure.link/WKIncludeFolder.jpg)

7. 回到起始目录，进入`Lib\<版本号，建议选最新>`文件夹，依次复制`ucrt/x64`、`um/x64`两个个文件夹的完整路径并追加到步骤4的`LIB`环境变量后

![WKLibFolder](https://c-environment.shigure.link/WKLibFolder.jpg)

8. 打开命令提示符，输入`cl`回车，若正常输出版本及提示信息则配置成功。

然后就可以在命令行使用MSVC编译器（cl）了。
</Details>

### CLion

`CLion`是`JetBrains`出品的`C/C++` IDE，功能强大，可惜没有免费版本，但是可以申请教育认证。（[安装包](https://www.jetbrains.com/clion/download/#section=windows)）

安装完成之后开箱即用。

## 手动配置

### 编译器选择

目前主流编译器有`GCC`、`Clang`、`MSVC`几种，这里只讲使用最多的`GCC`

1. 下载`MinGW`（Minimalist GNU for Windows）工具链（[下载](https://github.com/brechtsanders/winlibs_mingw/releases/download/14.2.0posix-19.1.1-12.0.0-msvcrt-r2/winlibs-x86_64-posix-seh-gcc-14.2.0-llvm-19.1.1-mingw-w64msvcrt-12.0.0-r2.zip)，由于是Github链接，可能会下不下来，自行寻找Github加速的方法）。
2. 将压缩包中的`mingw64`文件夹解压到任意地方，将`mingw64/bin`文件夹完整路径追加到`Path`环境变量之后。
3. 打开命令提示符，输入`gcc --version`回车，若正常输出版本则安装成功。

## 文本编辑器

任意文本编辑器都可以用于`C`编程，记事本都行。这里只讲较为常用的VSCode（Visual Studio Code）。VSCode是`Microsoft`家的文本编辑器，完全免费，还有一个社区的孪生兄弟`VSCodium`，是`VSCode`的开源版本。

1. [下载](https://code.visualstudio.com/docs/?dv=win64user)
2. 安装C语言插件（全名`C/C++`，是`Microsoft`官方的）。
3. 安装`Code Runner`用来运行程序

# 写在最后

至此，你已经有了一个能开发C语言的环境了。下一节，将正式开始`C`之旅。