---
title: 如何让Linux系统开机时自动挂载分区
id: 136
categories:
  - 技术点滴
date: 2014-12-11 11:29:29
tags:
---

最近在写chrome插件,每次开机的时候都得去挂载硬盘,然后导入插件,太麻烦了,于是想怎样让分区开机自动挂载,于是百度了下,有了以下方法

1.安装`ntfs-config`
``` bash
$ sudo apt-get install ntfs-config
```
<!--more-->
2.修改`NtfsConfig.py` 将56行的`os.mkdir` 改成`os.makedirs`
```bash
$ sudo vim /usr/lib/pymodules/python2.7/NtfsConfig/NtfsConfig.py
```

```bash
51         # Delete broken link. and create hal fdi directory if it don't exist
52         for value in (TARGET_NTFS_WRITE, TARGET_NTFS_READ) :
53             if not os.path.exists(value) and os.path.islink(value) :
54                 os.unlink(value)
55         if not os.path.exists(HAL_CONFIG_DIR) :

56             os.makedirs(HAL_CONFIG_DIR) #os.mkdir(HAL_CONFIG_DIR)
```

3.运行 `ntfs-config`
```bash
$ sudo ntfs-config 之后会出现如下界面。
```

[![ntfs-config ](https://old.itbugs.cn/wp-content/uploads/2014/12/QQ%E6%88%AA%E5%9B%BE20141211112539.png)](https://old.itbugs.cn/wp-content/uploads/2014/12/QQ%E6%88%AA%E5%9B%BE20141211112539.png)


选择你所需要的挂载对象，重启系统，将看到你所选定的挂载对象在系统起来的时候就被挂载上了.