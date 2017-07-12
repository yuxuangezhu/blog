---
title: input button onclick 链接
id: 99
categories:
  - 技术点滴
date: 2013-06-01 00:39:19
tags:
---

**input** **Button** 按钮，**跳转链接**，方法：

1.如果让本页**转向新的页面**则用：
```html
<input type=button onclick=”window.location.href(‘连接’)“>
```
2.如果需要**打开一个新的页面进行转向**，则用：
```html
<input type=button onclick=”window.open(‘连接’)“>
<input type=button value=刷新 onclick=”window.location.reload()“>
<input type=button value=前进 onclick=”window.history.go(1)“>
<input type=button value=刷新 onclick=”window.history.go(0)“>
<input type=button value=后退 onclick=”window.history.go(-1)“>
<input type=button value=前进 onclick=”window.history.forward()“>
<input type=button value=后退 onclick=”window.history.back()“>
```
后退+刷新
<!--more-->
```html
<input type=button value=后退 onclick=”window.history.go(-1);window.location.reload()“>
```
3.**打开无边框的新窗口**
```html
<input type="button" name="Submit2" value="确 定" onclick="javascript:(',','width=720,height=500,resizable=yes,scrollbars=yes,status=no')" />
```
4.点击按钮弹出确认alert窗口
方式一：
```html
<input type="button" name="Submit1" value="确定" onClick="alert('是否确认提交？');return false;" >
```
方式二：
```html
<input type="button" name="Submit2" value="确定" onClick="if (confirm('是否确认提交？'));return false;" >
```