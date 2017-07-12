---
title: centos6基于LNMP搭建gitlab ce教程
id: 231
categories:
  - 技术点滴
date: 2017-02-21 10:25:04
tags:
---

最近需要使用`gitlab`在内网进行版本管理，整了一台centos的服务器，版本6.8，因为已有LNMP存在，不想再使用gitlab自带的nginx配置，所以搜了一下教程，基本基于centos7的比较多，命令等执行起来多有失败，好在都解决了，所以整理了下centos6的安装方法，以备后用。

## 1、GitLab安装简介：

[GitLab安装说明](https://about.gitlab.com/downloads/)，按照官方的安装说明进行安装。

## 2、开始安装GitLab：

官方命令执行安装必备依赖
``` bash
sudo yum install curl openssh-server openssh-clients postfix cronie
sudo service postfix start
sudo chkconfig postfix on
sudo lokkit -s http -s ssh
```
安装完依赖之后开始正式安装gitlab，官方给出两种安装方式，这里我们使用[清华镜像站](https://mirror.tuna.tsinghua.edu.cn/help/gitlab-ce/)的源，选择指定yum源的方式进行安装

新建 `/etc/yum.repos.d/gitlab-ce.repo`，内容为

``` bash

  [gitlab-ce]
  name=gitlab-ce
  baseurl=http://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el6
  repo_gpgcheck=0
  gpgcheck=0
  enabled=1
  gpgkey=https://packages.gitlab.com/gpg.key
```
再执行
<!--more-->
``` bash
sudo yum makecache
sudo yum install gitlab-ce
```
一般情况下，不使用原有的LNMP环境的话，gitlab已经安装完成了，只需要执行
``` bash
sudo gitlab-ctl reconfigure
```
就可以直接启动gitlab了。

如果要基于LNMP的话，就需要对gitlab的默认配置进行修改
``` bash
vi /etc/gitlab/gitlab.rb
```
打开文件后，找到下面三处位置进行如下修改，有没启用的，需要自行删除前面的#
``` bash
# note the 'https' below
external_url 'https://域名orIP'

# Set the web server
web_server['external_users'] = ['www']

# Disable the built-in nginx
nginx['enable'] = false
```
修改完成后开始配置nginx
``` bash
vi /usr/local/nginx/conf/vhost/gitlab.conf
```
创建新文件，内容如下，部分地方需要修改
``` bash
## GitLab ##
## Modified from nginx http version
## Modified from http://blog.phusion.nl/2012/04/21/tutorial-setting-up-gitlab-on-debian-6/
## Modified from https://raymii.org/s/tutorials/Strong_SSL_Security_On_nginx.html ##
## Lines starting with two hashes (##) are comments with information.
## Lines starting with one hash (#) are configuration parameters that can be uncommented. ##
################################## ## CONTRIBUTING ## ##################################
## ## If you change this file in a Merge Request, please also create
## a Merge Request on https://gitlab.com/gitlab-org/omnibus-gitlab/merge_requests ##
################################### ## configuration ## ################################### ##
## See installation.md#using-https for additional HTTPS configuration details.
upstream gitlab-workhorse {
  server unix:/var/opt/gitlab/gitlab-workhorse/socket fail_timeout=0;
}
## HTTPS host
server {
  listen 80;
  listen 443 ssl;
  server_name 域名 or IP; ## Replace this with something like gitlab.example.com
  server_tokens off; ## Don't show the nginx version number, a security best practice
  root /opt/gitlab/embedded/service/gitlab-rails/public;
  include ssl.conf;
  access_log /tmp/gitlab.access.log;
  error_log /tmp/gitlab.error.log;
  location / {
    client_max_body_size 0;
    gzip off;

    ## https://github.com/gitlabhq/gitlabhq/issues/694
    ## Some requests take more than 30 seconds.
    proxy_read_timeout 300;
    proxy_connect_timeout 300;
    proxy_redirect off;
    proxy_http_version 1.1;
    proxy_set_header Host $http_host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-Ssl on;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_pass http:<span class="hljs-regexp">//gitlab-workhorse;
  }
}
```
其中所需要的`ssl.conf`文件 需要单独创建

内容如下
``` bash
ssl on;
add_header Strict-Transport-Security "max-age=31536000;includeSubdomains; preload;";
ssl_certificate /usr/local/nginx/conf/1_xxx.com.crt;
ssl_certificate_key /usr/local/nginx/conf/ssl.key;
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_ciphers ALL:!DH:!EXPORT:!RC4:+HIGH:+MEDIUM:!LOW:!aNULL:!eNULL;
ssl_session_cache shared:SSL:20m;
ssl_session_timeout 20m;
```
文件中的
`.crt`和`.key`文件需要自行去证书网站签发

上面文件准备好之后，

执行
``` bash
sudo gitlab-ctl reconfigure
```
使gitlab配置生效（每次更改配置都需要执行才能生效）

重启Nginx使配置生效

重启后访问刚才配置的server_name 就可以看见界面了

首次登陆需要设置root用户的密码，设置完成后就可以正常使用了



## 3、GitLab几个坑：

1.  很多时候我们安装完成，创建完项目会发现，系统给出的git clone的地址不对，有的时候是域名不对，有的时候是协议不对，这个时候就需要修改`vi  /etc/gitlab/gitlab.rb`中的 `gitlab_rails['gitlab_ssh_host'] = '域名 or IP'`，修改完成后执行
``` bash
sudo gitlab-ctl reconfigure
```
2.  如果你在上一步ssl证书的地方使用了不被认可的证书，使用git拉取代码的时候会报错
`
SSL: no alternative certificate subject name matches target host name '192.168.22.15'
`
这个时候需要执行clone命令之前先执行
``` bash
env GIT_SSL_NO_VERIFY=true
```
来忽略ssl警告，拉取完代码之后在当前仓库执行
``` bash
git config http.sslVerify <span class="string">"false"
```
可以对当前仓库永久忽略警告。

参考资料：
[http://blog.csdn.net/w670328683/article/details/50736977](http://blog.csdn.net/w670328683/article/details/50736977)
[http://schnell18.iteye.com/blog/2031397](http://schnell18.iteye.com/blog/2031397)
