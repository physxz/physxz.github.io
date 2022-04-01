---
title: 在Windows或Linux系统中编写并打包一个简单串口通信上位机
tags:
  - 上位机
  - Qt
categories:
  - 技术
index_img: /img/20220401-qt-serial.png
typora-root-url: ..
abbrlink: 10020
date: 2022-04-01 20:24:00
---

编写了一个简单的通过串口通信控制Arduino的桌面上位机App，在Windows和Linux系统下进行了打包。<!--more-->点击上位机的特定按钮，上位机通过串口发送"open"或"close"指令，Arduino收到指令后做出相应操作。

参考：[Windows编写及打包](https://zhuanlan.zhihu.com/p/367399556)，[Linux打包](https://zhuanlan.zhihu.com/p/49919048)。