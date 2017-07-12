---
title: LNMP下防跨站、跨目录安全设置，仅支持PHP 5.3.3以上版本
id: 95
categories:
  - 技术点滴
date: 2013-06-01 00:35:27
tags:
---

[LNMP一键安装包](http://lnmp.org/)下存在跨站和跨目录的问题，跨站和跨目录影响同服务器/VPS上的其他网站，最近看PHP 5.3，在**5.3.3以上**已经增加了HOST配置，可以起到防跨站、跨目录的问题。

如果你是PHP 5.3.3以上的版本，可以修改`/usr/local/php/etc/php.ini`在末尾里加入：
```bash
[HOST=www.vpser.net]
open_basedir=/home/wwwroot/www.vpser.net/:/tmp/
[PATH=/home/wwwroot/www.vpser.net]
open_basedir=/home/wwwroot/www.vpser.net/:/tmp/
```
按上面的这个例子修改，换成你自己的域名和目录，多个网站就按上面的例子改成多个，最后重启
```bash
php-fpm：/etc/init.d/php-fpm restart
```
如果让网站可以使用探针需要在/tmp/后加上:/proc/
<!--more-->
PHP 5.3.3以上版本的用户，可以执行：
```bash
cd /root;rm -f /root/vhost.sh;wget http://soft.vpser.net/lnmp/ext/vhost.sh;chmod +x /root/vhost.sh
```
这样替换原来的vhost.sh文件，以后添加网站就会自动添加HOST防跨站、跨目录的配置。

为解决升级PHP 5.3.*版本后部分需要PHP 5.2.*版本的程序无法运行的问题，我们会增加一个PHP 5.2的安装脚本，脚本将在未来几天发布。

如有问题或与更好的方法，欢迎在本文或到[论坛](http://bbs.vpser.net/)[lnmp区](http://bbs.vpser.net/forum-25-1.html)反馈。

**&gt;&gt;转载请注明出处：[VPS侦探](http://www.vpser.net/) 本文链接地址：[http://www.vpser.net/security/lnmp-cross-site-corss-dir-security.html](http://www.vpser.net/security/lnmp-cross-site-corss-dir-security.html "LNMP下防跨站、跨目录安全设置，仅支持PHP 5.3.3以上版本")**