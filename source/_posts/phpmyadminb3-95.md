---
title: 解决phpmyadmin上传文件大小限制的配置方法
id: 61
categories:
  - 技术点滴
date: 2013-05-29 23:16:15
tags:
---

phpmyadmin导入SQL文件时涉及到phpmyadmin上传文件大小限制问题，默认phpmyadmin上传文件大小为2M，如果想要phpmyadmin上传超过2M大文件，就需要修改phpmyadmin上传文件的大小配置或者将大文件分几批上传，相对来说修改phpmyadmin上传文件大小的限制来得方便很多。解决phpmyadmin上传文件大小限制问题涉及修改php.ini配置文件和phpmyadmin配置文件。

修改phpmyadmin上传文件大小限制主要分修改php.ini配置文件和phpmyadmin配置文件两个步骤。

**第一步：修改php.ini配置文件中文件上传大小配置**

此步骤与一般的PHP.INI配置文件上传功能方法一致，需要修改php.ini配置文件中**upload_max_filesize**和**post_max_size**两个选项值，具体修改方法请参考：[PHP.INI配置:文件上传功能配置教程](http://www.leapsoul.cn/?p=488)。

**第二步：修改php执行时间及内存限制实现phpmyadmin上传大文件功能**

如果想要phpmyadmin上传大文件，还需修改php.ini配置文件中的max_execution_time（php页面执行最大时间）、max_input_time（php页面接受数据最大时间）、memory_limit（php页面占用的最大内存）三个配置选项，这是因为phpmyadmin上传大文件时，php页面的执行时间、内存占用也势必变得更长更大，其需要php运行环境的配合，光修改上传文件大小限制是不够的。

**第三步：修改phpmyadmin配置文件**

在完成php.ini的相关配置后，还需要修改phpmyadmin配置。

1、修改phpmyadmin config配置文件中的**$cfg['ExecTimeLimit']**配置选项，默认值是300，需要修改为0，即没有时间限制。
<!--more-->
2、修改phpmyadmin安装根目录下的import页面中的**$memory_limit
```php
$memory_limit = trim(@ini_get('memory_limit'));
// 2 MB as default
if (empty($memory_limit)) {
    $memory_limit = 2 * 1024 * 1024;
}
// In case no memory limit we work on 10MB chunks
if ($memory_limit == -1) {
    $memory_limit = 10 * 1024 * 1024;
}
```
**说明**：首选读取php.ini配置文件中的内存配置选项memory_limit，如果为空则默认内存大小限制为2M，如果没有限制则内存大小限制为10M，你可以结合你php.ini配置文件中的相关信息修改这段代码。

至此，经过修改php.ini配置文件中的文件上传配置选项以及phpmyadmin配置文件后，即可解决phpmyadmin上传文件大小限制问题，从而实现phpmyadmin上传大文件功能。

**注**：[PHP网站开发教程-leapsoul.cn](http://www.leapsoul.cn/ "PHP网站开发-PHP教程-leapsoul - 分享PHP网站开发与建设的乐趣,以PHP实例教程方式教你建站")版权所有，转载时请以链接形式注明原始出处及本声明，谢谢。