---
title: 免费https证书申请
id: 159
categories:
  - 技术点滴
date: 2015-04-01 14:55:34
tags:
---

今天刷微博,无意中看到一篇介绍https的文章,看完后决定给自己的博客升升级,也整个https高大上一下.

于是开始百度免费的ssl证书提供商,发现大家都在说国内的好点,于是点了一个看着还不错的国内网站 https://buy.wosign.com/ ,看了一下,百度经验的文章已然过期了,网站流程改了,没发现能免费申请,页面上挂的那么多好几千的吓得我赶紧就要叉掉它,就在这时突然发现了一个暴漫

![](https://buy.wosign.com/images/V2/a4.jpg)

暴漫底下有这么一块货

![](https://buy.wosign.com/images/V2/a3.jpg)

这尼玛不就是免费的吗,果断注册去领(领完才发现直接给的是个不知道咋用的邮件证书),至于我们最关心的网站证书,要去个人中心点下上方的购买证书,然后拉到最低有个免费的DV SSL证书的申请,点击申请就好了,一步一步的个人感觉没啥特殊要求的话直接默认各个选项就好

签名算法:
``` bash
SHA1 (按照国际标准规定,SHA1证书有效期不能超过2017年1月1日.)

SHA2 (更安全，但WinXP SP2不支持)
```
这一块的话,`SHA1`支持度比较好,`SHA2`是新标准,有些地方支持度不是很好,我要求不高,又嫌麻烦,所以果断选了一次申请三年的SHA2

工作时间申请完了大约10分钟就能接到结果,通过邮件取回生成的证书,取回证书时需要填写一个密码,这个密码是用来下载之后解压的,拿到压缩包之后会发现有多种服务器的证书版本,我用的`Nginx`服务器,于是就选了for Nginx的版本,把压缩包里的两个文件上传到服务器,我是放在`/home/ssl/`里面的,具体放哪随意,文件夹和文件要`www`权限.


下面是具体的安装过程，当然`SSL证书`的安装跟博客程序（不管是`wordpress`或者`typecho`等等）无关，只是跟服务器的类型（比如`Nginx`、`Apache`或者`IIS`等）有关。
``` bash
  ----------操作说明----------               
  系统：            CentOS 6.5
  环境：            LNMP（只需配置Nginx服务器就行）
  操作工具：        linux 终端
  博客程序：        wordpress
```
<!--more-->
我们需要修改Nginx的配置文件 
`/usr/local/nginx/conf/nginx.conf`
来让`Nginx`启用`https`协议，示例如下：
执行
``` bash
# vi nginx.conf
```
按下面对文件进行修改
``` bash
###### 新增一个“server”，保留原80端口，并强制将http协议转换到https协议 

server {
  listen 80;
  server_name itbugs.cn;
  rewrite ^(.*) https://$server_name$1 permanent; 
}
```
在原“server”中，启用支持https协议的443端口，并配置相关信息
``` bash
server {
  listen 443; server_name itbugs.cn;
  index index.html index.htm index.php default.html default.htm default.php;
  root /home/www/yuxuan;
  ssl on;
  # 下载的.crt文件
  ssl_certificate /home/ssl/typecodes_last.crt;
  #下载的.key文件
  ssl_certificate_key /home/ssl/typecodes.key;
  ssl_session_cache shared:SSL:10m;
  ssl_session_timeout 10m;
  ssl_ciphers ALL:!aNULL:!ADH:!eNULL:!LOW:!EXP:RC4+RSA:+HIGH:+MEDIUM; 
  ssl_protocols SSLv3 TLSv1;
  ssl_prefer_server_ciphers on;
***************省略其它不变的部分****************
}
```
重启Nginx服务
这时访问 http://itbugs.cn 都会自动跳转到 https://itbugs.cn 了。因此，大功告成！