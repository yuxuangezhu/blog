---
title: ubuntu 无法删除 deepin-media-player deepin-music-player
id: 102
categories:
  - 技术点滴
date: 2013-06-06 23:33:30
tags:
---

下列软件包将被【卸载】：

deepin-media-player deepin-music-player

升级了 0 个软件包，新安装了 0 个软件包， 要卸载 2 个软件包，有 0 个软件包未被升级。

有 2 个软件包没有被完全安装或卸载。

解压缩后将会空出 50.0 MB 的空间。

您希望继续执行吗？[Y/n] y

(正在读取数据库 ... 系统当前共安装有 201353 个文件和目录。)

正在卸载 deepin-media-player ...

rm: 无法删除"/usr/share/deepin-media-player": 没有那个文件或目录

dpkg: error processing deepin-media-player (--remove):

子进程 已安装 post-removal 脚本 返回了错误号 1

由于已经达到 MaxReports 限制，没有写入 apport 报告。

正在卸载 deepin-music-player ...

rm: 无法删除"/usr/share/deepin-music-player": 没有那个文件或目录

dpkg: error processing deepin-music-player (--remove):
<!--more-->
子进程 已安装 post-removal 脚本 返回了错误号 1

由于已经达到 MaxReports 限制，没有写入 apport 报告。

在处理时有错误发生：

deepin-media-player

deepin-music-player

E: Sub-process /usr/bin/dpkg returned an error code (1)

&nbsp;

解决办法：

cd /var/lib/dpkg

sudo mv info{,.bak}

sudo mkdir info

sudo dpkg --configure -a

sudo apt-get install -f

卸载成功！

![](http://a.hiphotos.baidu.com/album/pic/item/7af40ad162d9f2d3974e7487a8ec8a136327cc7f.jpg)