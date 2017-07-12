---
title: 日常 shell 命令汇总
id: 190
categories:
  - 技术点滴
date: 2016-03-16 10:28:04
tags:
---

> 1.检查远程端口是否对bash开放：
```bash
echo >/dev/tcp/8.8.8.8/53 && echo "open"
```
> 2.让进程转入后台：
```bash
Ctrl + z
```
> 3.将进程转到前台：
```bash
fg
```
<!--more-->
> 4.产生随机的十六进制数，其中n是字符数：
```bash
openssl rand -hex n
```
> 5.在当前shell里执行一个文件里的命令：
```bash
source /home/user/file.name
```
> 6.截取前5个字符：
```bash
${variable:0:5}
```
> 7.SSH debug 模式:
```bash
ssh -vvv user@ip_address
```
> 8.SSH with pem key:
```bash
ssh user@ip_address -i key.pem
```
> 9.用wget抓取完整的网站目录结构，存放到本地目录中：
```bash
wget -r --no-parent --reject "index.html*" http://hostname/ -P /home/user/dirs
```
> 10.一次创建多个目录：
```bash
mkdir -p /home/user/{test,test1,test2}
```
> 11.列出包括子进程的进程树：
```bash
ps axwef
```
> 12.创建 war 文件:
```bash
jar -cvf name.war file
```
> 13.测试硬盘写入速度：
```bash
dd if=/dev/zero of=/tmp/output.img bs=8k count=256k; rm -rf /tmp/output.img
```
> 14.测试硬盘读取速度：
```bash
hdparm -Tt /dev/sda
```
> 15.获取文本的md5 hash：
```bash
echo -n "text" | md5sum
```
> 16.检查xml格式：
```bash
xmllint --noout file.xml
```
> 17.将tar.gz提取到新目录里：
```bash
tar zxvf package.tar.gz -C new_dir
```
> 18.使用curl获取HTTP头信息：
```bash
curl -I http://www.example.com
```
> 19.修改文件或目录的时间戳(YYMMDDhhmm):
```bash
touch -t 0712250000 file
```
> 20.用wget命令执行ftp下载：
```bash
wget -m ftp://username:password@hostname
```
> 21.生成随机密码(例子里是16个字符长):
```bash
LANG=c < /dev/urandom tr -dc _A-Z-a-z-0-9 | head -c${1:-16};echo;
```
> 22.快速备份一个文件：
```bash
cp some_file_name{,.bkp}
```
> 23.访问Windows共享目录：
```bash
smbclient -U "DOMAIN\user" //dc.domain.com/share/test/dir
```
> 24.执行历史记录里的命令(这里是第100行):
```bash
!100
```
> 25.解压:
```bash
unzip package_name.zip -d dir_name
```
> 26.输入多行文字(CTRL + d 退出):
```bash
cat > test.txt
```
> 27.创建空文件或清空一个现有文件：
```bash
> test.txt
```
> 28.与Ubuntu NTP server同步时间：
```bash
ntpdate ntp.ubuntu.com
```
> 29.用netstat显示所有tcp4监听端口：
```bash
netstat -lnt4 | awk '{print $4}' | cut -f2 -d: | grep -o '[0-9]*'
```
> 30.qcow2镜像文件转换：
```bash
qemu-img convert -f qcow2 -O raw precise-server-cloudimg-amd64-disk1.img \precise-server-cloudimg-amd64-disk1.raw
```
> 31.重复运行文件，显示其输出（缺省是2秒一次）：
```bash
watch ps -ef
```
> 32.所有用户列表：
```bash
getent passwd
```
> 33.Mount root in read/write mode:
```bash
mount -o remount,rw /
```
> 34.挂载一个目录（这是不能使用链接的情况）:
```bash
mount --bind /source /destination
```
> 35.动态更新DNS server:
```bash
nsupdate < <EOF update add $HOST 86400 A $IP send EOF
```
> 36.递归grep所有目录：
```bash
grep -r "some_text" /path/to/dir
```
> 37.列出前10个最大的文件：
```bash
lsof / | awk '{ if($7 > 1048576) print $7/1048576 "MB "$9 }' | sort -n -u | tail
```
> 38.显示剩余内存(MB):
```bash
free -m | grep cache | awk '/[0-9]/{ print $4" MB" }'
```
> 39.打开Vim并跳到文件末：
```bash
vim + some_file_name
```
> 40.Git 克隆指定分支(master):
```bash
git clone git@github.com:name/app.git -b master
```
> 41.Git 切换到其它分支(develop):
```bash
git checkout develop
```
> 42.Git 删除分支(myfeature):
```bash
git branch -d myfeature
```
> 43.Git 删除远程分支
```bash
git push origin :branchName
```
> 44.Git 将新分支推送到远程服务器：
```bash
git push -u origin mynewfeature
```
> 45.打印历史记录中最后一次cat命令：
```bash
!cat:p
```
> 46.运行历史记录里最后一次cat命令：
```bash
!cat
```
> 47.找出/home/user下所有空子目录:
```bash
find /home/user -maxdepth 1 -type d -empty
```
> 48.获取test.txt文件中第50-60行内容：
```bash
< test.txt sed -n '50,60p'
```
> 49.运行最后一个命令(如果最后一个命令是`mkdir /root/test`, 下面将会运行: `sudo mkdir /root/test`)：
```bash
sudo !!
```
> 50.创建临时RAM文件系统 – ramdisk (先创建/tmpram目录):
```bash
mount -t tmpfs tmpfs /tmpram -o size=512m
```
> 51.Grep whole words:
```bash
grep -w "name" test.txt
```
> 52.在需要提升权限的情况下往一个文件里追加文本：
```bash
echo "some text" | sudo tee -a /path/file
```
> 53.列出所有kill signal参数:
```bash
kill -l
```
> 54.在bash历史记录里禁止记录最后一次会话：
```bash
kill -9 $$
```
> 55.扫描网络寻找开放的端口：
```bash
nmap -p 8081 172.20.0.0/16
```
> 56.设置git email:
```bash
git config --global user.email "me@example.com"
```
> 57.To sync with master if you have unpublished commits:
```bash
git pull --rebase origin master
```
> 58.将所有文件名中含有”txt”的文件移入/home/user目录:
```bash
find -iname "*txt*" -exec mv -v {} /home/user \;
```
> 59.将文件按行并列显示：
```bash
paste test.txt test1.txt
```
> 60.shell里的进度条:
```bash
pv data.log
```
> 61.使用netcat将数据发送到Graphite server:
```bash
echo "hosts.sampleHost 10 `date +%s`" | nc 192.168.200.2 3000
```
> 62.将tabs转换成空格：
```bash
expand test.txt > test1.txt
```
> 63.Skip bash history:
```bash
< space >cmd
```
> 64.去之前的工作目录：
```bash
cd -
```
> 65.拆分大体积的tar.gz文件(每个100MB)，然后合并回去：
```bash
split –b 100m /path/to/large/archive /path/to/output/files cat files* > archive
```
> 66.使用curl获取HTTP status code:
```bash
curl -sL -w "%{http_code}\\n" www.example.com -o /dev/null
```
> 67.设置root密码，强化MySQL安全安装:
```bash
/usr/bin/mysql_secure_installation
```
> 68.当Ctrl + c不好使时:
```bash
Ctrl + \
```
> 69.获取文件owner:
```bash
stat -c %U file.txt
```
> 70.block设备列表：
```bash
lsblk -f
```
> 71.找出文件名结尾有空格的文件：
```bash
find . -type f -exec egrep -l " +$" {} \;
```
> 72.找出文件名有tab缩进符的文件
```bash
find . -type f -exec egrep -l $'\t' {} \;
```
> 73.用”=”打印出横线:全选复制放进笔记
```bash
printf '%100s\n' | tr ' ' =
```