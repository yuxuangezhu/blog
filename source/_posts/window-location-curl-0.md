---
title: window.location操作URL属性的函数
id: 168
categories:
  - 技术点滴
date: 2015-04-10 17:46:34
tags:
---

最近在做网站的时候，为了提高URL信息的利用,需要对url数据进行操作,于是百度了下发现主要就是**获取当前URL的详细信息**，就可以进行判断了，这就需要用到**下列函数**

设置或获取对象指定的文件名或路径。
window.location.pathname

设置或获取整个 URL 为字符串。
window.location.href

设置或获取与 URL 关联的端口号码。
window.location.port

设置或获取 URL 的协议部分。
window.location.protocol

设置或获取 href 属性中在井号“#”后面的分段。
window.location.hash
<!--more-->
设置或获取 location 或 URL 的 hostname 和 port 号码。
window.location.host

设置或获取 href 属性中跟在问号后面的部分。
window.location.search

## <a name="t1"></a>window.location

<table width="100%" border="0" cellspacing="0" cellpadding="0">
<thead>
<tr>
<th align="left">属性</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>hash</td>
<td>设置或获取 href 属性中在井号“#”后面的分段。</td>
</tr>
<tr>
<td>host</td>
<td>设置或获取 location 或 URL 的 hostname 和 port 号码。</td>
</tr>
<tr>
<td>hostname</td>
<td>设置或获取 location 或 URL 的主机名称部分。</td>
</tr>
<tr>
<td>href</td>
<td>设置或获取整个 URL 为字符串。</td>
</tr>
<tr>
<td>pathname</td>
<td>设置或获取对象指定的文件名或路径。</td>
</tr>
<tr>
<td>port</td>
<td>设置或获取与 URL 关联的端口号码。</td>
</tr>
<tr>
<td>protocol</td>
<td>设置或获取 URL 的协议部分。</td>
</tr>
<tr>
<td>search</td>
<td>设置或获取 href 属性中跟在问号后面的部分。</td>
</tr>
</tbody>
</table>