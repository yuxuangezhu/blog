---
title: UBUNTU 搜狗输入法崩溃 FCITX崩溃无痛重启方法
id: 154
categories:
  - 技术点滴
date: 2015-03-17 17:17:13
tags:
---

Ubuntu的搜狗输入法bug还是多多啊，比如总有那么几次，fcitx的cpu占用率到了100%，就听到cpu风扇呼呼呼地转。或者偶尔直接提示你崩掉了，让你重启。

注销有时能解决问题，可是一旦注销了，所有打开的程序都关了。这里给一种无伤的重启fcitx方法。


1 首先top，列出进程表，找到fcitx的pid

或者直接pidof fcitx
<!--more-->
2 sudo kill 掉fcitx

3  fcitx &amp;  这里的意思是后台跑fcitx，等出现loader time之类的字样的时候，就可以Ctrl+C终止了，

4  sogou-qimpanel &amp;  同样，后台启动搜狗输入法面板,同样可以Ctrl+C终止

此时就可以切换输入法试试看了