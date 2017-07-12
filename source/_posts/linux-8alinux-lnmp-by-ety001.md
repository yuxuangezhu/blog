---
title: linux分区知识及linux下配置lnmp by ETY001
id: 36
categories:
  - 心语心寻
date: 2013-05-28 13:11:02
tags:
---

你可以把 / 看作是windows的我的电脑，
```bash
/root
/home
/dev
/var
/etc
```
之类的文件夹，你可以暂时把他想象为C盘，D盘之类的各个硬盘分区
一般用户信息会存放在 “/home/用户名/”  这个目录下

所谓的用户信息，就是你这个用户平时使用电脑时的word文档，音乐，电影，游戏and so on
`root`用户是例外
`root`用户的家目录就是`/root`

只有`root`用户可以对根目录（也就是/）下的所有文件和文件夹进行读写操作。
其他的非`root`用户，一般情况默认只能向`/home/用户名/`目录下写文件，也就是他只能操作自己的文件夹空间。

比如说，我如果新建一个`ety001`的用户，那么默认情况下，`ety001`这个用户只有`/home/ety001/`目录下的所有操作权限
这里就涉及到另外一个必备的知识，用户和用户组
每个文件和文件夹都有自己的所属用户和用户组。
<!--more-->
```bash
[gallery ids="37"]
```
看这个就是用`ls -al`命令显示的

最前面的d，表示是dictionary目录。

r表示read，读，w表示写，x表示可执行
d后面是九列
你可以看到倒数第二行
`wwwlogs`是一个目录

这九列，每3列分一组

第一组表示当前目录所属用户的权限，第二列表示当前目录所属用户组的权限，第三列表示当前目录其他用户的操作权限

然后数字后面两列，第一列表示这个目录的所属用户，第二列表示所属用户组
所以说`vpn`目录是属于`root`用户的，也是属于`root`用户组的，`root`用户对于这个目录可读可写可执行
`root`组的用户对这个目录是可读可执行不可写，

其他的用户对这个目录也是可读可执行不可写

改变一个目录及其子目录的所属用户的命令是`chown -R 目录名 用户名`(具体是先用户名还是目录名的等百度吧~)

我的建议是，登陆后，先用`passwd`改下密码
然后
```bash
cd /home
mkdir lnmp
cd lnmp
```
刚才上面我和你说的那几个命令就是实现把`lnmp`的安装脚本下载到`/home/lnmp`目录下
你先下载那个lnmp的安装脚本吧
我今天先手把手教你搭建起lnmp

恩。
你切换到`/home/lnmp`
然后执行 
```bash
wget -c http://soft.vpser.net/lnmp/lnmp0.8-full.tar.gz
```
`wget`是linux下超好用的下载工具。

一般只要wget 要下载的文件地址 就可以了。

参数c貌似是可以断点续传吧，我百度下。

另外，还有个小技巧，tab键可以自动补全。

比如说，你在home目录下，这个目录里有一个lnmp目录，你想输入`cd lnmp`，那么你输入`cd l`后，可以按tab键自动补全，如果home目录下没有其他l开头的目录，系统就会自动给你补全

你如果在lnmp目录下的话，可以执行下ls命令
list的缩写，列目录的意思

再就是如果你想获取某个命令的帮助的话，可以输入man list，这样就能打开list命令的帮助文件
```bash
tar zxvf lnmp0.8-full.tar.gz
```
这时你可以用下自动补全
输入
```bash
tar zxvf l
```
然后按tab
zxvf是tar命令的参数
tar是tar包的压缩和解压缩命令。

解压完后，你再ls下

**3.CentOS下安装步骤**
下载版执行命令 `cd lnmp0.8/` ，完整版执行命令：`cd lnmp0.8-full/`
然后再执行`./centos.sh` 也可以执行`./centos.sh | tee lnmp.log` (推荐这种方式,出错时可以到[论坛](http://bbs.vpser.net/forum-25-1.html) 上传lnmp.log日志)，输入要绑定的域名(建议使用一个二级域名,该域名会绑定到/home/wwwroot/)，回车，再输入要设置的`MySQL root`的密码，回车，提示"Press any key to start..."，按任意键开始安装。程序会自动安装编译Nginx、PHP、MySQL、phpMyAdmin、Zend这几个软件。
我建议你输入你的不带www的域名

然后能安装完lnmp后，再去nginx的虚拟主机配置文件里添加上www.myacmroad.tk

安装过程很漫长。。。
慢慢等吧

 1、安装eAccelerator，执行如下命令：`./eaccelerator.sh` ，按提示选择版本，回车确认后，就会自动安装并重启web服务。

2、5、安装PureFTPd和管理面板，执行如下命令：./pureftpd.sh 按提示输入你MySQL的root密码、FTP用户管理面板的密码、MySQl的FTP数据库密码(可直接回车，自动生成一个密码)，回车确认，就会自动安装PureFTPd，安装完PureFTPd，在浏览器执行http://你的域名或IP/ftp/ 输入你前面设置的FTP用户管理面板的密码，就可以管理。

第一个你可以百度下看看作用，基本上是提高效率的

第二个是ftp服务器。

以后，你需要去学习下vi或者vim

因为在linux命令行下，是使用这个进行编辑配置文件的。

其实只要懂这几条命令就可以了。
`:q`   退出
`：wq!`  强制保存并退出

再就是在i，进入插入模式，esc退回到命令模式