---
title: nodejs运行稳定性提高方法
id: 170
categories:
  - 技术点滴
date: 2015-04-14 10:21:05
tags:
---

最近在写websocket的即时通讯工具，后台用nodejs写的，测试的时候开着ssh运行着也没多大问题，但是随着项目慢慢结尾，就出现问题了，工具需要node在后台持续运行着，但是我一关闭ssh链接，过一会，node就崩了，完全没法用啊，于是请教了万能的度娘，找到了下面这个方法；

用node启动server后，发现服务器不稳定，经常crash。我是用ssh远程登录的，ssh远程通道中断，或者Ctrl+C,都会使nodejs server崩溃掉。

**nohup的解决办法**

**1，启动node server**
```bash
[root@yuxuan nodejs]# nohup node server.js >> /var/log/nodejs/server_port_8020.log &
```
启动node server，并放到后台执行，并且记录log日志，注意：这样记录日志，时间长了，日志文件会比较大，要自己写一个shell脚本，控制文件大小。
```bash
[root@yuxuna nodejs]# nohup node server.js 1>/dev/null 2>&;1 & //这种方式，不记录log日志。
```
注意：用nohup的方法，node server是没有守护进程的，放到后台运行，如果node server崩溃掉，web一样不能访问。

**2，关闭node server**
```bash
[root@yuxuan ~]# ps aux|grep node    //查看node server
root     10680  0.0  0.3 826308 14556 pts/6    Sl   12:20   0:00 node server.js
root     15765  0.0  0.7 1031248 30144 ?       Sl   Feb25   0:01 node scripts/web-server.js
root     19648  0.0  0.0 103240   872 pts/3    S+   13:57   0:00 grep node

[root@yuxuan ~]# kill 10680   //关闭10680的node server

[root@hatch ~]# ps aux|grep node
root     15765  0.0  0.7 1031248 30144 ?       Sl   Feb25   0:01 node scripts/web-server.js
root     19653  0.0  0.0 103240   872 pts/3    S+   13:57   0:00 grep node
```
**三，forever工具**
<!--more-->
**1，安装forever**
```bash
# npm install forever
```
forever目前只支持到node 0.8，如果装了node 0.10的，forever工具就不能用了。安装的时候会报出 `warning forever wants 0.8, but has 0.10` ，虽然能安装成功，但是用不了。

**2，forever参数和用法**
```bash
# forever [start | stop | stopall | list] [options] SCRIPT [script options]
```
**3，forever实例**
```bash
# forever start server.js    //启动node server

# forever list                    //查看启动的node server
  0 server.js [ 24597, 24596 ]

# forever stop 0                  //停止第0个node server
Forever stopped process:
  0 server.js [ 24611, 24596 ]
```
关于**forever**的安装，目测可能会出现两个错误
1、`npm ERR! Error: SSL Error: CERT_UNTRUSTED`
解决方案：升级您的版本的NPM
```bash
npm install npm -g --ca=""
```
或告诉你的当前版本的NPM使用已知的
```bash
npm config set ca ""
```

2、`Error: No compatible version found:×××××`
如果在Linux系统中，通过一条命令更新npm可以解决。
```bash
npm install -g npm
```
在Window环境中，必须升级NPM到1.4.10以上的版本，我重新安装了node-v0.10.31-x64.msi，对应的NPM为1.4.23，再此运行npm install命令，依赖包下载运行正常。
