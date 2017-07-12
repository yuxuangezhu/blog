---
title: 页面关闭或刷新触发事件
id: 164
categories:
  - 技术点滴
date: 2015-04-09 14:40:44
tags:
---

最近在研究websocket,想在用户关闭或切换页面时向服务器发送一个用户id,并提醒用户要离开,
<!--more-->
网上有很多代码,这里我在百度的代码里找到了一段。
```javascript
window.onbeforeunload = function(event){
    var message = {
        "user":localStorage.userName,
        "uid":uid,
        "messType":"pageClose"
    }
    socket.send(message); //发送信息到服务器
    console.log(1111)
    return "提示文本"
}
```