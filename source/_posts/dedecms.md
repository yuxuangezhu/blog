---
title: 制作织梦dedecms首页RSS订阅源
id: 83
categories:
  - 技术点滴
date: 2013-06-01 00:17:36
tags:
---

织梦CMS默认情况下，RSS订阅源是根据分类区分不同的RSS订阅的。如果用户想订阅整个网站的RSS是个麻烦事，下面给出解决办法：

1、添加一个RSS模板，文件名为:rss_index.htm,将RSS模板文件保存到/templets/plus/目录下。文件内容为：
``` xml
<?xml version="1.0" encoding="{dede:global.cfg_soft_lang /}" ?>
<rss version="2.0">
<channel>
<title>{dede:global.cfg_webname/}</title>
<link>{dede:global.cfg_basehost/}</link>
<description>{dede:global.cfg_description/}</description>
<language>zh_cn</language>
<generator>{dede:global.cfg_webname/}</generator>
<webmaster>{dede:global.cfg_adminemail/}</webmaster>
{dede:arclist row='50' orderby='pubdate' titlelen='200'}
<item>
<title><![CDATA[[field:title/]]]></title>
<link>[field:arcurl/]</link>
<category>[field:typename/]</category>
<pubdate>[field:pubdate function='strftime("%a,%d%b%Y%H:%M:%S +0800",@me)'/]</pubdate>
<description><![CDATA[[field:array runphp='yes']@me = (strpos(@me['litpic'],'defaultpic') ? "": "<a [email=href='%7B@me[%22arcurl%22]%7D']href='{@me["arcurl"]}'[/email] target='_blank'><img [email=src='%7B@me[%22litpic%22]%7D']src='{@me["litpic"]}'[/email] border='0' /><br />"); [/field:array][field:description function='html2text(@me)'/] ... ]]></description>
</item>
{/dede:arclist}
</channel>
</rss>
```
<!--more-->
2、在根目录中添加rss.php文件，文件内容为：
``` php
<?php
require_once (dirname(__FILE__) . "/include/common.inc.php");
require_once DEDEINC."/arc.partview.class.php";
$pv = new PartView();
$pv->SetTemplet($cfg_basedir . $cfg_templets_dir . "/plus/rss_index.htm");
header("Content-type:application/xml");
$pv->Display();
?>
```
3、在首页index.htm模板的头部标签中添加属性，代码如下：
``` html
<link rel=”alternate” type=”application/rss+xml” title=”{dede:field.title/}” href=”http://www.dedecms.com/rss.php“/>
```
然后重新生成静态，通过浏览器上的RSS源识别按钮即可识别。或者在网页其它地方添加A标签形式的订阅链接。 默认情况下，最多调用50个最新的文章，它有个好处时，是动态文件，不需要每次生成，只要有新文章，RSS就会更新的。