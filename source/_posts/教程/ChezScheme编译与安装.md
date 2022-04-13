---
title: ChezScheme编译与安装
date: 2018-02-12 20:32:24
tags: 教程
---

Chez Scheme是传说中最好的scheme实现。虽然它已经开源了，~~但是貌似还没有出现在apt软件源中。如果想安装的话，就只能自己编译了~~ 。

现在ubuntu软件仓库中已经有Chez Scheme的软件包了，直接`sudo apt install chezscheme`就能安装，`scheme`就能打开使用。所以这篇博客下面的内容作废。


## 1. 安装依赖

首先我们需要安装一些需要用到的软件，用于下载源代码的版本控制工具git,用于编译的gcc和make,以及最重要的ncurses库和X windows库，没有这俩就会编译失败。

#### ubuntu

``` bash
sudo apt install git gcc make libncurses5-dev libx11-dev
```

#### CentOS

``` bash
yum install -y git gcc make ncurses-devel libX11-devel

```

## 2. 下载源码

由于源代码的完整的git仓库比较大，下载所需时间较长，所以只下载最近1次提交

``` bash
git clone https://github.com/cisco/ChezScheme.git --depth 1
```

## 3. 编译与安装

这一步需要的时间与网速和电脑的配置有关

``` bash
./configure && sudo make install
```

## 4. 使用

直接用`scheme`即可打开ChezScheme解释器

![](/img/ChezScheme.png)

然后就可以愉(tong)快(ku)的写代码啦