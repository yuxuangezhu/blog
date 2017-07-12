---
title: 把数据保存到数据库主表 `#@__archives` 时出错,请把相关信息提交给DedeCms官方。
id: 66
categories:
  - 技术点滴
date: 2013-05-31 15:00:05
tags:
---

把数据保存到数据库主表 `#@__archives` 时出错,请把相关信息提交给DedeCms官方。

使用织梦5.6的用户升级到5.7的时候经常会出现这种错误，这时使用后台SQL命令行就可以

情况一：运行
```bash
ALTER TABLE `#@__archives` ADD COLUMN `voteid` int(10) NOT NULL DEFAULT 0 AFTER `mtype`;
```
请况二：查看`@__archives`表结构发现不同。我的语句就变成这个
```bash
ALTER TABLE `#@__archives` ADD COLUMN `weight` int(10) NOT NULL DEFAULT 0 AFTER `voteid`;
```
这个是在sql执行，完了就可以，所以重点还是要看表结构是不是有啥不同。