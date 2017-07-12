---
title: Linux版OpenVPN安装、配置教程
id: 152
categories:
  - 技术点滴
date: 2015-03-17 11:29:44
tags:
---

本文将以目前最新的openvpn-2.3.4.tar.gz(更新于2014-5-2，[下载地址](http://www.softown.cn/soft/openvpn/linux))为例来介绍如何在Linux系统中安装、配置及使用OpenVPN。

在这里，我们选用了一台预装CentOS 6.5 64位系统的计算机作为OpenVPN服务器，一台预装Ubuntu 14.04 64位系统的计算机作为OpenVPN客户端，来演示Linux版OpenVPN的安装配置操作过程。实际上，OpenVPN并不区分Linux发行版本，几乎所有的配置都是一样的，因此Red Hat、Fedora、SUSE等Linux发行版均可参考本文。

### 安装OpenVPN的前提条件

在安装OpenVPN之前你必须确保你已经安装了C编译器(例如gcc)、OpenSSL、LZO(一种无损压缩算法)、PAM(一种可插入式的身份验证模块)。如果你安装了yum，可以使用如下命令来安装这些工具(选择性地安装之前没有安装的软件包即可)：
```bash
#安装gcc、openssl、lzo、pam
yum install -y gcc openssl-devel lzo-devel pam-devel
```
> Ubuntu、Debian等系统使用apt-get进行安装，安装命令请[参考这里](http://www.softown.cn/post/142.html)。

### 安装Linux版OpenVPN

首先，我们需要先下载OpenVPN安装程序，上面已经给出了Linux版OpenVPN的下载地址，在这里我们就不多说了。

在这里，我们将下载的安装文件移动到`/usr/local/`文件夹中(你也可以自行移动到其他目录)。

[![OpenVPN安装文件](http://img.softown.cn/attach/image/201405/linux-install-openvpn-1.png "OpenVPN安装文件")](http://img.softown.cn/attach/image/201405/linux-install-openvpn-1.png "OpenVPN安装文件")

接着，我们使用`tar`命令，将该压缩文件解压到当前目录，完整命令为：
```bash
tar zxf openvpn-2.3.4.tar.gz
```
<!--more-->
[![解压后的OpenVPN目录](http://img.softown.cn/attach/image/201405/linux-install-openvpn-2.png "解压后的OpenVPN目录")](http://img.softown.cn/attach/image/201405/linux-install-openvpn-2.png "解压后的OpenVPN目录")

接着，我们依次执行如下命令：
```bash
#跳转到解压后的openvpn目录
cd openvpn-2.3.4
#调用configure
./configure
#编译
make
#安装
make install
```
### OpenVPN配置

安装OpenVPN完毕后，接下来就应该配置OpenVPN，以便于其能正常工作。配置OpenVPN主要有两个步骤：一是生成服务器和客户端所需的各种证书，二是编写服务器和客户端所需的配置文件。

#### 下载`easy-rsa`

想要生成各种证书和密钥，我们还要用到easy-rsa(只有服务器端需要easy-rsa，客户端无需安装)。坑爹的是，OpenVPN 2.3.x并没有自带这个东西，我们还需要去[GitHub下载easy-rsa](https://github.com/OpenVPN/easy-rsa)。更坑爹的是，GitHub上的easy-rsa已经升级到了3.0版本，该版本几乎重写了之前所有的程序代码，连使用方法都全变了，然而OpenVPN的官方文档并没有作相应更新，其中介绍的仍然是easy-rsa 2.0的操作方法。为了避免不必要的麻烦，我们推荐下载2.0版本的easy-rsa，你也可以直接[点击这里](http://pan.baidu.com/s/1o66Yaz4)下载。

在这里我们将下载的easy-rsa-release-2.x.zip文件放在/root目录中。我们依次执行如下命令：
```bash
#转到easy-rsa安装文件所在目录
cd /root/
#解压该安装文件
unzip -q easy-rsa-release-2.x.zip
```
执行结果如下图所示(其中的ls命令用于查看文件列表)：

[![解压easy-rsa](http://img.softown.cn/attach/image/201405/linux-install-openvpn-3.png "解压easy-rsa")](http://img.softown.cn/attach/image/201405/linux-install-openvpn-3.png "解压easy-rsa")

接着，我们将上图中所标注的easy-rsa-release-2.x/easy-rsa文件夹复制到OpenVPN的解压目录中，命令如下：
```bash
#复制解压后的easy-rsa目录到OpenVPN解压目录下
cp -r easy-rsa-release-2.x/easy-rsa /usr/local/openvpn-2.3.4
```
[![复制easy-rsa](http://img.softown.cn/attach/image/201405/linux-install-openvpn-4.png "复制easy-rsa")](http://img.softown.cn/attach/image/201405/linux-install-openvpn-4.png "复制easy-rsa")

然后，我们执行命令
```bash
cd /usr/local/openvpn-2.3.4/easy-rsa/2.0
```
从而进入OpenVPN下的easy-rsa/2.0目录。

[![easy-rsa的2.0目录文件列表](http://img.softown.cn/attach/image/201405/linux-install-openvpn-5.png "easy-rsa的2.0目录文件列表")](http://img.softown.cn/attach/image/201405/linux-install-openvpn-5.png "easy-rsa的2.0目录文件列表")
> 上面说了这么多，实际上就只是下载了easy-rsa 2.0，并将解压后的easy-rsa子目录复制到了OpenVPN的主目录下。这里的文件夹2.0就是我们以后生成各种证书和密钥的根据地了。

#### 使用easy-rsa生成CA证书

在生成证书之前，我们建议你对2.0目录中的vars文件稍作修改。vars文件存储的是一些用户变量设置信息，每次生成证书都会使用到其中的某些变量。如下图所示，我们着重建议你关注红色矩形框内的变量，并选择性地对其进行修改(你可以不修改这些参数，但不要把这些参数留空)。
> KEY_SIZE：表示密钥的长度，一般为1024或2048(长度越长，性能耗费越多)。
> 
> 
> #下面是一些用户相关信息配置
> 
> KEY_COUNTRY：所在国家
> 
> KEY_PROVINCE：所在省
> 
> KEY_CITY：所在城市
> 
> KEY_ORG：所在组织
> 
> KEY_EMAIL：邮箱地址
> 
> KEY_OU：机构单位或部门名称
[![vars文件部分内容](http://img.softown.cn/attach/image/201405/linux-install-openvpn-6.png "vars文件部分内容")](http://img.softown.cn/attach/image/201405/linux-install-openvpn-6.png "vars文件部分内容")

下面，我们就开始来生成证书了。保持当前目录为OpenVPN根目录/easy-rsa/2.0。然后依次执行下列命令：
```bash
#初始化命令，用于设置后续命令所需的相关变量信息
. ./vars
#清除之前创建的所有证书和密钥
./clean-all
#生成CA证书和密钥
./build-ca
```
> 注意：证书的用户信息可以根据需要自行输入。如果不输入、直接回车，则表示该字段使用"[]"中的默认值(也就是前面vars文件中设置的参数值)；如果输入"."，则表示该字段留空。在这里需要注意Common Name字段，这相当于证书的"用户名"，请确保每个证书的Common Name字段是唯一的。
[![执行初始化命令并创建CA证书](http://img.softown.cn/attach/image/201405/linux-install-openvpn-7.png "执行初始化命令并创建CA证书")](http://img.softown.cn/attach/image/201405/linux-install-openvpn-7.png "执行初始化命令并创建CA证书")

到这里，我们的CA证书和密钥就已经生成成功了，生成的证书和密码默认均存放在当前目录的子文件夹keys中。

#### 生成服务器端证书

接下来，我们为服务器和客户端生成各自所需的证书和密钥(所有的证书和密钥都必须由OpenVPN服务器上的easy-rsa生成)。

我们可以执行命令
```bash
./build-key-server server
```
来生成服务器端所需的证书和密钥。如下图所示，与创建CA证书一样，我们先输入证书的相关信息，并在最后输入两次"y"确认生成即可。

[![生成server端证书和密钥](http://img.softown.cn/attach/image/201405/linux-install-openvpn-8.png "生成server端证书和密钥")](http://img.softown.cn/attach/image/201405/linux-install-openvpn-8.png "生成server端证书和密钥")

#### 生成客户端证书

与创建服务器端证书类似，我们可以使用命令./build-key clientName来生成客户端证书和密钥，其中clientName为自定义的客户端名称(例如：client1、client2、jim、tom)。如果需要为多个客户端生成证书，只需要分别执行多次即可。

[![生成客户端证书和密钥](http://img.softown.cn/attach/image/201405/linux-install-openvpn-9.png "生成客户端证书和密钥")](http://img.softown.cn/attach/image/201405/linux-install-openvpn-9.png "生成客户端证书和密钥")

#### 生成迪菲·赫尔曼交换密钥

此外，我们还需要为OpenVPN的服务器端创建迪菲·赫尔曼交换密钥，命令为`./build-dh`(无需额外输入，耐心等待生成完毕即可)。迪菲·赫尔曼交换密钥是一种安全协议，用以对数据进行加密。

[![生成迪菲·赫尔曼交换密钥](http://img.softown.cn/attach/image/201405/linux-install-openvpn-10.png "生成迪菲·赫尔曼交换密钥")](http://img.softown.cn/attach/image/201405/linux-install-openvpn-10.png "生成迪菲·赫尔曼交换密钥")

#### 生成TLS-auth密钥

这一步骤是可选操作。OpenVPN提供了TLS-auth功能，可以用来抵御Dos、UDP端口淹没攻击。出于安全考虑，你可以启用该功能；启用该功能，你需要执行命令
```bash
openvpn --genkey --secret keys/ta.key
```
来生成TLS-auth所需的密钥文件。

到这里，我们的证书生成就告一段落了。如果你以后想要生成新的客户端或执行其他操作，只需要先执行命令`. ./vars`，然后执行相应的命令即可，例如`./build-key client2`。

最后，我们来看看我们一共生成了哪些证书和密钥。
> 编号①：CA证书和密钥
> 
> 编号②：客户端client1的证书和密钥，
> 
> 编号③：迪菲·赫尔曼交换密钥 (如果你的KEY_SIZE=1024，则该文件名称为dh1024.pem)。
> 
> 编号④：服务器端证书和密钥。
> 
> 编号⑤：启用tls-auth所需的文件。
[![生成的所有证书文件和密钥](http://img.softown.cn/attach/image/201405/linux-install-openvpn-11.png "生成的所有证书文件和密钥")](http://img.softown.cn/attach/image/201405/linux-install-openvpn-11.png "生成的所有证书文件和密钥")

#### 编写配置文件

众所周知，OpenVPN虽然可以分为客户端和服务器，不过它们的安装程序是完全一样的，只是通过不同的证书和配置文件来进行区分。在这里，我们先在OpenVPN主目录下创建一个config目录，并将其所需的证书和密钥文件拷贝到该目录中。

其中，服务器端需要用到的文件有：
ca.crt
ca.key
dh2048.pem (如果最初的变量KEY_SIZE设为1024，这里就是dh1024.pem)
server.crt
server.key
ta.key (如果不开启tls-auth，则无需该文件)
客户端client1需要用到的文件有：
ca.crt
client1.crt
client1.key (名称client1根据个人设置可能有所不同)
ta.key (如果不开启tls-auth，则无需该文件)
在这里，我们以OpenVPN服务器端为例来演示上述操作流程。
```bash
#创建config目录
mkdir /usr/local/openvpn-2.3.4/config
#复制证书和密钥文件到config目录
cp keys/ca.crt keys/ca.key keys/server.crt keys/server.key keys/dh2048.pem keys/ta.key /usr/local/openvpn-2.3.4/config
```
[![拷贝证书和密钥文件](http://img.softown.cn/attach/image/201405/linux-install-openvpn-12.png "拷贝证书和密钥文件")](http://img.softown.cn/attach/image/201405/linux-install-openvpn-12.png "拷贝证书和密钥文件")

此外，我们还需要为服务器和每个客户端的config目录分别编写一个配置文件，服务器端的配置文件为server.conf，客户端的配置文件为client.conf。

这两个配置文件该如何编写呢？OpenVPN已经在sample/sample-config-files子目录中为我们提供了相关的示例文件server.conf和client.conf，并且配置文件中的每个配置选项均有详细的英文说明(配置文件中"#"或";"开头的均为注释内容)。

[![OpenVPN提供的示例配置文件](http://img.softown.cn/attach/image/201405/linux-install-openvpn-13.png "OpenVPN提供的示例配置文件")](http://img.softown.cn/attach/image/201405/linux-install-openvpn-13.png "OpenVPN提供的示例配置文件")

现在，我们先将server.conf文件拷贝到config目录中，然后再对其进行修改。
```bash
#转到simple-config-files目录
cd /usr/local/openvpn-2.3.4/sample/sample-config-files
#复制server.conf到config目录中
cp server.conf /usr/local/openvpn-2.3.4/config
```
[![复制到config目录中](http://img.softown.cn/attach/image/201405/linux-install-openvpn-14.png "复制到config目录中")](http://img.softown.cn/attach/image/201405/linux-install-openvpn-14.png "复制到config目录中")

在这里，我们先给出server.conf的详细配置，并注明每项配置的作用。
```bash
local 192.168.1.106     #指定监听的本机IP(因为有些计算机具备多个IP地址)，该命令是可选的，默认监听所有IP地址。
port 1194             #指定监听的本机端口号
proto udp             #指定采用的传输协议，可以选择tcp或udp
dev tun               #指定创建的通信隧道类型，可选tun或tap
ca ca.crt             #指定CA证书的文件路径
cert server.crt       #指定服务器端的证书文件路径
key server.key    #指定服务器端的私钥文件路径
dh dh2048.pem         #指定迪菲赫尔曼参数的文件路径
server 10.0.0.0 255.255.255.0   #指定虚拟局域网占用的IP地址段和子网掩码，此处配置的服务器自身占用10.0.0.1。
ifconfig-pool-persist ipp.txt   #服务器自动给客户端分配IP后，客户端下次连接时，仍然采用上次的IP地址(第一次分配的IP保存在ipp.txt中，下一次分配其中保存的IP)。
tls-auth ta.key 0     #开启TLS-auth，使用ta.key防御攻击。服务器端的第二个参数值为0，客户端的为1。
keepalive 10 120      #每10秒ping一次，连接超时时间设为120秒。
comp-lzo              #开启VPN连接压缩，如果服务器端开启，客户端也必须开启
client-to-client      #允许客户端与客户端相连接，默认情况下客户端只能与服务器相连接
persist-key
persist-tun           #持久化选项可以尽量避免访问在重启时由于用户权限降低而无法访问的某些资源。
status openvpn-status.log    #指定记录OpenVPN状态的日志文件路径
verb 3                #指定日志文件的记录详细级别，可选0-9，等级越高日志内容越详细
接着是客户端配置文件client.conf。
client         #指定当前VPN是客户端
dev tun        #必须与服务器端的保持一致
proto udp      #必须与服务器端的保持一致
remote 192.168.1.106 1194      #指定连接的远程服务器的实际IP地址和端口号
resolv-retry infinite    #断线自动重新连接，在网络不稳定的情况下(例如：笔记本电脑无线网络)非常有用。
nobind         #不绑定特定的本地端口号
persist-key
persist-tun
ca ca.crt      #指定CA证书的文件路径
cert client1.crt       #指定当前客户端的证书文件路径
key client1.key    #指定当前客户端的私钥文件路径
ns-cert-type server      #指定采用服务器校验方式
tls-auth ta.key 1     #如果服务器设置了防御DoS等攻击的ta.key，则必须每个客户端开启；如果未设置，则注释掉这一行；
comp-lzo              #与服务器保持一致
verb 3                #指定日志文件的记录详细级别，可选0-9，等级越高日志内容越详细
```
> 实际上，将两个模板文件中与IP地址有关的配置修改一下，就可以直接拿来使用。
> 
> 关于OpenVPN配置文件的更多信息请参考[server.conf配置详解](http://www.softown.cn/post/137.html)和[client.conf配置详解](http://www.softown.cn/post/138.html)。

#### 启动OpenVPN

当我们把服务器和客户端所需的证书、密钥和配置文件都分配完毕之后，我们就可以尝试启动OpenVPN来检查我们的工作成果了。

在Linux中，我们可以直接执行以下命令来启动OpenVPN：
openvpn 配置文件路径
如果你是服务器端，就指定server.conf文件的路径，如果你是客户端就指定client.conf文件的路径。

请注意：

1.  配置文件中的文件路径涉及到相对路径的，均以启动OpenVPN时的所在目录为准。由于我们在配置文件中设置的文件路径都是相对config目录的路径，因此我们也只能在config目录下才能正常启动OpenVPN。如果你想在任何地方都能使用上述命令启动OpenVPN，建议你将配置文件与文件路径相关的部分全部改为绝对路径。
2.  OpenVPN服务器所在计算机必须允许OpenVPN通过防火墙，你可以禁用掉防火墙，或者将OpenVPN设为可信程序，或者开放1194端口。
3.  启动服务器和客户端都需要一定的权限，建议用root账户或`sudo`命令进行启动。
以下就是OpenVPN服务器的启动效果：

[![启动OpenVPN服务器成功](http://img.softown.cn/attach/image/201405/linux-install-openvpn-15.png "启动OpenVPN服务器成功")](http://img.softown.cn/attach/image/201405/linux-install-openvpn-15.png "启动OpenVPN服务器成功")

客户端的启动效果如下：

[![启动OpenVPN客户端成功](http://img.softown.cn/attach/image/201405/linux-install-openvpn-16.png "启动OpenVPN客户端成功")](http://img.softown.cn/attach/image/201405/linux-install-openvpn-16.png "启动OpenVPN客户端成功")

我们在客户端尝试`ping`服务器的虚拟IP地址10.0.0.1，顺利`ping`通。

[![OpenVPN客户端ping服务器IP地址](http://img.softown.cn/attach/image/201405/linux-install-openvpn-17.png "OpenVPN客户端ping服务器IP地址")](http://img.softown.cn/attach/image/201405/linux-install-openvpn-17.png "OpenVPN客户端ping服务器IP地址")

作者：[软件指南针](http://www.softown.cn/post/140.html)(http://www.softown.cn)，转载请保留出处！