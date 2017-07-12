---
title: 预装win8，更改win7的机器
id: 69
categories:
  - 技术点滴
date: 2013-05-31 15:02:54
tags:
---

## 预装win8，更改win7的机器，注意了

1、开机点击F1进入到bios界面
2、进入`Security—Secure Boot—Disabled`
如果不修改`Secure boot选项为Disabled`，在光驱引导时可能会出现报错
3、进入`Startup—UEFI/Legacy Boot`选项
`UEFI/Legacy Boot`选项选择成`Both`，`UEFI/Legacy Boot Priority`选择成`UEFI First`