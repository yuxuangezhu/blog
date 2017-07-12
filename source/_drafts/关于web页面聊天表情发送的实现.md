---
title: 关于web页面聊天表情发送的实现
id: 156
categories:
  - 心语心寻
tags:
---

最近在写一个web的聊天工具,鉴于聊天的时候不能发表情简直蛋疼,于是考虑加入表情功能,搜了一下,大多数教程都要引入js库,于是决定自己写一个.

首先创建两个数组:

face_box = ["smilea_org.gif", "tootha_org.gif", "laugh.gif", "tza_org.gif", "kl_org.gif", "kbsa_org.gif"];
face_name = ["[呵呵]", "[嘻嘻]", "[哈哈]", "[可爱]", "[可怜]", "[挖鼻屎]"];

然后将表情选择