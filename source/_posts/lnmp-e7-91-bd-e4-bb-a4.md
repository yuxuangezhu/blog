---
title: LNMP相关目录和命令
id: 93
categories:
  - 技术点滴
date: 2013-06-01 00:34:10
tags:
---

安装：
```bash
wget -c http://soft.vpser.net/lnmp/lnmp0.9-full.tar.gz
tar zxvf lnmp0.9-full.tar.gz
cd lnmp0.9-full/
```

确认Linux发行版：
```bash
cat /etc/issue
```

CentOS系统下的安装
```bash
./centos.sh 2&gt;&amp;1 | tee lnmp.log
```

Debian系统下的安装
```bash
./debian.sh 2&gt;&amp;1 | tee lnmp.log
```

Ubuntu系统下的安装
```bash
./ubuntu.sh 2&gt;&amp;1 | tee lnmp.log
```
<!--more-->
添加虚拟主机
```bash
/root/vhost.sh
```

删除虚拟主机
```bash
rm /usr/local/nginx/conf/vhost/quany.info.conf
```

安装组件
安装PureFTPd和FTP管理面板：
```bash
./pureftpd.sh
```

安装eAccelerator：
```bash
./eaccelerator.sh
```

安装ionCube：
```bash
./ionCube.sh
```

安装imageMagick：
```bash
./imageMagick.sh
```

安装memcached：
```bash
./memcached.sh
```

升级
升级Nginx：
```bash
./upgrade_nginx.sh
```

升级PHP版本：
```bash
./upgrade_php.sh

状态管理
LNMP状态管理：
```bash
/root/lnmp {start|stop|reload|restart|kill|status}

Nginx状态管理：
```bash
/etc/init.d/nginx {start|stop|reload|restart}
```

PHP-FPM状态管理：
```bash
/etc/init.d/php-fpm {start|stop|quit|restart|reload|logrotate}
```

PureFTPd状态管理：
```bash
/etc/init.d/pureftpd {start|stop|restart|kill|status}
```

MySQL状态管理：
```bash
/etc/init.d/mysql {start|stop|restart|reload|force-reload|status}
```

Memcached状态管理：
```bash
/etc/init.d/memcached {start|stop|restart}
```

相关图形界面程序
> phpinfo: `http://www.quany.info/phpinfo.php`
> 
> phpMyAdmin: `http://www.quany.info/phpmyadmin/`
> 
> 探针: `http://www.quany.info/p.php`
> 
> PureFTP管理界面: `http://www.quany.info/ftp/`
> 
> Memcached测试页面: `http://www.quany.info/memcached.php`
LNMP相关目录
> nginx: `/usr/local/nginx`
> 
> mysql: `/usr/local/mysql`
> 
> php: `/usr/local/php`
> 
> 网站目录: `/home/wwwroot/`
> 
> Nginx日志目录: `/home/wwwlogs/`
> 
> `/root/vhost.sh`添加的虚拟主机配置文件所在目录: `/usr/local/nginx/conf/vhost/`
LNMP相关配置文件
> Nginx主配置文件: `/usr/local/nginx/conf/nginx.conf`
> 
> `/root/vhost.sh`添加的虚拟主机配置文件: `/usr/local/nginx/conf/vhost/域名.conf`
> 
> MySQL配置文件: `/etc/my.cnf`
> 
> PHP配置文件: `/usr/local/php/etc/php.ini`
> 
> php-fpm配置文件: `/usr/local/php/etc/php-fpm.conf`
> 
> PureFtpd配置文件: `/usr/local/pureftpd/pure-ftpd.conf`
> 
> PureFtpd MySQL配置文件: `/usr/local/pureftpd/pureftpd-mysql.conf`