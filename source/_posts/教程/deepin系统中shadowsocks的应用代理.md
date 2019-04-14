---
title: 在deepin系统中使用shadowsocks代理
date: 2018-02-20 21:11:02
tags: 教程
---

近期，我在deepin系统上用jetbrains家的idea进行web开发。由于某墙，idea加载maven依赖的速度非常慢，无法忍受的我使用shadowsocks代理进行翻墙。

## 1. 安装shadowsocks-qt5

打开深度商店，搜索“shadowsocks-qt5”并安装
此软件如何使用请自行百度
![](assets/img/deepin-shadowsocks-0.png)


## 2. 配置应用代理

打开“设置”-“网络设置”-“应用代理”
![](assets/img/deepin-shadowsocks-2.png)

代理类型选择socks5
ip地址填127.0.0.1
端口填1080
用户名和密码不用填
点确定

![](assets/img/deepin-shadowsocks-3.png)

## 3. 使用

以idea为例
在应用启动器中找到idea，右击打开一个列表，然后在“使用代理打开”前打上钩

![](assets/img/deepin-shadowsocks-4.png)

然后就设置好了，打开idea会看到加载依赖的速度快到飞起
