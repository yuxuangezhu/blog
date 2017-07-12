---
title: 关于移动开发touch滑动的简单实现
id: 141
categories:
  - 技术点滴
date: 2015-08-26 11:57:22
tags:
---

今天在写页面，用到了触摸滑动，百度了下，发现框架有很多，但是没什么单独的，想到对于只用上下触摸事件的我来说，引入一个框架总感觉有点鸡肋，于是尝试着自己写了一种方法，感觉还不错。

代码如下
<!--more-->
``` javascript
var pageY=0;
var pageY1=0;
var pageY2=0;
$(document).mousedown(function(e){//获取点击时的鼠标坐标
    //$("p").text(e.pageX + ", " + e.pageY);
    pageY1 = e.pageY;
});

$(document).mouseup(function(e){//获取鼠标松开时的鼠标坐标
    //$("p").text(e.pageX + ", " + e.pageY);
    pageY2 = e.pageY;
    pageY = pageY1-pageY2;
    if (pageY == 0) {
        return false;
    } else if (pageY < 10) {
        moveDown();
    } else if (pageY > -10) {
        moveUp();
    }
});
```