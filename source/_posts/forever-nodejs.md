---
title: 使用forever运行nodejs应用
id: 208
categories:
  - 技术点滴
date: 2016-06-20 10:40:06
tags:
---

forever可以看做是一个nodejs的守护进程，能够启动，停止，重启我们的app应用。

官方的说明是说：

A simple CLI tool for ensuring that a given script runs continuously (i.e. forever).
// 一个用来持续（或者说永远）运行一个给定脚本的简单的命令行工具

Github地址：`https://github.com/nodejitsu/forever`
用途

forever的用途就是帮我们更好的管理我们node App服务，本质上就是在forever进程之下，创建一个node app的子进程。

比如，你有一个基于express的或者其他的一些个应用那么，它将会很方便你更新和操作你的服务，并且保证你服务能持续运行。

更好的一点就是每次更改文件，它都可以帮你自动重启服务而不需要手动重启。
安装forever
``` bash
sudo npm install forever -g // 记得加-g，forever要求安装到全局环境下
```
forever使用说明
启动相关
<!--more-->
1\. 简单的启动
``` bash
forever start app.js
```

2\. 指定forever信息输出文件，当然，默认它会放到`~/.forever/forever.log`
``` bash
forever start -l forever.log app.js
```
3\. 指定app.js中的日志信息和错误日志输出文件，
  -o 就是console.log输出的信息，`-e`就是`console.error`输出的信息
``` bash
forever start -o out.log -e err.log app.js
```

4\. 追加日志，forever默认是不能覆盖上次的启动日志，
  所以如果第二次启动不加`-a`，则会不让运行
``` bash
forever start -l forever.log -a app.js
```

5\. 监听当前文件夹下的所有文件改动
``` bash
forever start -w app.js
```

文件改动监听并自动重启

1\. 监听当前文件夹下的所有文件改动（不太建议这样）
``` bash
forever start -w app.js
```

2\. 显示所有运行的服务
``` bash
forever list
```
停止操作

1\. 停止所有运行的`node App`
``` bash
forever stopall
```
2\. 停止其中一个`node App`
``` bash
forever stop app.js
```
当然还可以这样
  forever list 找到对应的id，然后：
``` bash
forever stop [id]
```

重启操作

重启操作跟停止操作保持一致。

1\. 启动所有
```
forever restartall
```

更多一些

上面的一些解释足够平常使用，还有待之后继续补充。
开发和线上建议配置
``` bash
// 开发环境下
NODE_ENV=development forever start -l forever.log -e err.log -a app.js
// 线上环境下
NODE_ENV=production forever start -l ~/.forever/forever.log -e ~/.forever/err.log -w -a app.js
```
上面加上`NODE_ENV`为了让`app.js`辨认当前是什么环境用的。不加它可能就不知道哦？
一些注意点

有可能你需要使用unix下的`crontab`（定时任务）

这个时候需要注意配置好环境变量。
``` bash
SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
```
后记

毋庸置疑，拥有了Github就拥有了世界。
参考

https://github.com/nodejitsu/forever
http://blog.nodejitsu.com/keep-a-nodejs-server-up-with-forever
https://github.com/nodejitsu/forever/issues/116