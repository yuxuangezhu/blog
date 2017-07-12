---
title: SSH命令详解
id: 75
categories:
  - 技术点滴
date: 2013-05-31 15:07:55
tags:
---
```bash
rm -rf mydir /* 删除mydir目录 */

cd mydir /* 进入mydir目录 */

cd - /* 回上一级目录 */

cd ~ /* 回根目录 */

mv tools tool /* 把tools目录改名为tool */

ln -s tool bac

/* 给tool目录创建名为bac的符号链接,最熟悉的应该就是FTP中www链接到public_html目录了 */

cp -a tool /home/leavex/www /* 把tool目录下所有文件复制到www目录下 */

rm go.tar /* 删除go.tar文件 */

find mt.cgi /* 查找文件名为mt.cgi的文件 */

df –h /* 查看磁盘剩余空间,好像没这个必要，除非你太那个了 */

tar xvf wordpress.tar /* 解压tar格式的文件 */

tar -tvf myfile.tar /* 查看tar文件中包含的文件 */
<!--more-->
gzip -d ge.tar.gz /* 解压.tar.gz文件为.tar文件 */

unzip phpbb.zip /* 解压zip文件，windows下要压缩出一个.tar.gz格式的文件还是有点麻烦的 */

tar cf toole.tar tool /* 把tool目录打包为toole.tar文件 */

tar cfz geek.tar.gz tool

/* 把tool目录打包且压缩为geek.tar.gz文件，因为.tar文件几乎是没有压缩过的，MT的.tar.gz文件解压成.tar文件后差不多是10MB */

wget

wget http://www.example.com/download/wp.tar.gz

/*下载远程服务器上的文件到自己的服务器，连上传都省了，服务器不是100M就是1000M的带宽，下载一个2-3兆的MT还不是几十秒的事 */

wget -c http://www.example.com/undone.zip

/* 继续下载上次未下载完的文件 */

tar cfz xxxx.tar.gz tool

/* 把tool目录打包且压缩为xxxx.tar.gz文件，因为.tar文件几乎是没有压缩过的，MT的.tar.gz文件解压成.tar文件后差不多是10MB */
```
还有一些是VIM里要用到的，也罗列出来吧!

移动类的：
```bash
h/j/k/l: 左/下/上/右　移一格

w : 向后词移动　(前面加数字移动多少个词)

b : 向前词移动　(前面加数字移动多少个词)

e : 向后移到词末

ge : 向前移到词末

$ : 行末

0 : 行首

tx : 向右查找本行的x并移到那儿(大写时向左)

33G : 移到文件的第33行

gg : 文件首行

G : 文件尾行

33% : 文件的33%处

H/M/L : 屏幕的首/中/尾行

zt/zz/zb : 当前行移到屏幕的首/中/底部
```
跳转：
```bash
” : 回到跳转来的地方

CTRL-O : 跳到一个 “较老” 的地方

CTRL-I : 则跳到一个 “较新” 的地方
```
查找：
```bash
/ : 向下查找(后加关键字)

? : 向上查找(后加关键字)

n : 下一条符合的记录
```
编辑：
```bash
i : 转换到插入模式

x : 删除当前字符

. : 重复最后一次的修改操作(同PS里ctrl+f执行滤镜)

u : 撤销操作

CTRL-R : 重做

p : 将删除的字符插入到当前位置(put)
```
退出保存：
```bash
:q : 退出

:q! : 不保存退出

ZZ : 保存后退出

:e! : 放弃修改重新编辑
```
退出SSH后，继续运行!
```bash
#nohup wget http://www.example.net/file.tar.gz &amp;
```
wget是一个Linux环境下用于从World Wide Web上提取文件的工具，这是一个GPL许

可证

下的自由软件，其作者为Hrvoje Niksic 。wget支持HTTP和

FTP协议

，支持代理服务器和断点续传功能，能够自动递归远程主机的目录，找到合乎条件

的文

件并将其下载到本地硬盘上;如果必要，wget将恰当地转换页面中的超级连接以在

本地

生成可浏览的镜像。由于没有交互式界面，wget可在后台运行，截获并忽略

HANGUP信号

，因此在用户推出登录以后，仍可继续运行。通常，wget用于成批量地下载

Internet网

站上的文件，或制作远程网站的镜像。

语法:
```bash
wget [options] [URL-list]
```
URL地址格式说明:可以使用如下格式的URL:

http://host[:port]/path

例如:

http://fly.cc.fer.hr/

ftp://ftp.xemacs.org/pub/xemacs/xemacs-19.14.tar.gz

ftp://username:password@host/dir/file

在最后一种形式中，以URL编码形式为FTP主机提供了用户名和密码(当然，也可以

使用

参数提供该信息，见后)。

参数说明：

wget的参数较多，但大部分应用只需要如下几个常用的参数：

-r 递归;对于HTTP主机，wget首先下载URL指定的文件，然后(如果该文

件是

一个HTML文档的话)递归下载该文件所引用(超级连接)的所有文件(递归深度由

参数

-l指定)。对FTP主机，该参数意味着要下载URL指定的目录中的所有文件，递归方

法与

HTTP主机类似。

-N 时间戳：该参数指定wget只下载更新的文件，也就是说，与本地目录中

的对

应文件的长度和最后修改日期一样的文件将不被下载。

-m 镜像：相当于同时使用-r和-N参数。

-l 设置递归级数;默认为5。-l1相当于不递归;-l0为无穷递归;注意，

当递

归深度增加时，文件数量将呈指数级增长。

-t 设置重试次数。当连接中断(或超时)时，wget将试图重新连接。如

果指

定-t0，则重试次数设为无穷多。

-c 指定断点续传功能。实际上，wget默认具有断点续传功能，只有当你使

用别

的ftp工具下载了某一文件的一部分，并希望wget接着完成此工作的时候，才需要

指定

此参数。

使用举例：

wget -m -l4 -t0 http://www.php100.com/

将在本地硬盘建立http://www.example.com/的镜像，镜像文件存入当前目录下一个

名为

oneweb.com.cn的子目录中(你也可以使用-nH参数指定不建立该子目录，而直接在

当前

目录下建立镜像的目录结构)，递归深度为4，重试次数为无穷(若连接出现问题

，

wget将坚韧不拔地永远重试下去，知道任务完成!)

另外一些使用频率稍低的参数如下：

-A acclist / -R rejlist：

这两个参数用于指定wget接受或排除的文件扩展名，多个名称之间用逗号隔开。例

如，

假设我们不想下载MPEG视频影像文件和.AU声音文件，可使用如下参数：

-R mpg,mpeg,au

其它参数还有：

-L 只扩展相对连接，该参数对于抓取指定站点很有用，可以避免向宿主

主机

的其他目录扩散。例如，某个人网站地址为：http://www.example.com/~ppfl/，使用

如下

命令行：

wget -L http://www.example.com/~ppfl/

则只提取该个人网站，而不涉及主机www.example.com上的其他目录。

-k 转换连接：HTML文件存盘时，将其中的非相对连接转换成为相对连接。

-X 在下载FTP主机上的文件时，排除若干指定的目录

另外，下面参数用于设置wget的工作界面：

-v 设置wget输出详细的工作信息。

-q 设置wget不输出任何信息。

如果我们已经在一个HTML文档(或普通文本文档)中存储了所要提取的文件的连接

，可

以让wget直接从该文件中提取信息，而不用在命令行中提供URL地址，参数格式为

：

-i filename

地址文件也可以不是HTML文档，例如，一个普通的文本文件，其中有需要下载的

URL列

表即可。

我们可以用以下技巧提高下载速度：由于Linux是一个多任务系统，我们可以同时

运行

多个wget进程以提高下载速度，例如，先下载某主页文件(index.html)，然后将

该文

件所列出的所有地址分别用一个独立的wget进程进行下载。

至于其他的参数，可参考wget的man手册页，命令为：man wget

用wget创建网站的镜像

使用shell中的wget命令行创建网站镜像的方法。此方法将所有文件(包括图片、CSS等)都下载下来，并把网页中的链接改为相对链接，这样就避免了镜像中的链接仍旧指向原来的网站而不能正常地工作了。

此方法只需一条命令行：

de&gt;$ wget -mk -w 20 http://www.example.com/de&gt;

命令行中的20代表间隔20秒下载一个文件，这样可以避免网站的访问过于频繁。你可以调小点，但当你是备份别人的站时，还是为别人的服务器考虑下吧。

SSH下载

用SSH下载文件，大家应该都会了吧?

那如何上传呢??

以下情况可能会用到上传。。

假设我在dreamhost里做了个站，发展不错。内容也很多，但是访问速度肯定不如国内了，所以我准备把数据都搬回国内。这时我们肯定要先用SSH打包备份了。

远程SSH打包命令如下：

tar cfz geek.tar.gz tool

/* 把tool目录打包且压缩为geek.tar.gz文件，因为.tar文件几乎是没有压缩过的，MT的.tar.gz文件解压成.tar文件后差不多是10MB */

压缩打包好了，要转移到新服务器上，传统方法是用登录FTP，然后下载压缩包，再登录新服务器上传压缩包。

这个时候，如果文件小还好，文件要是很大的话，这一下一上肯定费不少时间。很是麻烦。

其实，利用SSH，可以直接把文件上传到远程服务器上。下面给大家举例子：

假设我的压缩包在code/mwpk.tar.gz 而远程服务器IP qmun.com 用户：user 密码:123456

我们首先登录SSH。

并且转到code目录下。

cd code /*转到code目录

ls /*列出该目录所有文件

下面就是利用SSH上传的命令了。

[lenny]$ ftp /*启用FTP客户端

ftp&gt; open qmun.com /*打开远程服务器IP

Connected to qmun.com.

220 ProFTPD 1.2.9 Server ready.

Name (qmun.com:root): user /*输入用户名

331 Password required for oran.

Password: /*输入密码

230 User oran logged in.

put mwpk.tar.gz mwpk.tar.gz

/*这是关键，put是上传命令，第一个mwpk.tar.gz是本地文件名，第二个是远程文件名。。意思就是把本地的mwpk.tar.gz上传到远程FTP里，并且命名为mwpk.tar.gz这样，SSH就可以自己上传了