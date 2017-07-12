---
title: node后台运行
id: 206
categories:
  - 心语心寻
tags:
---

1.用forever  进行管理
<table class="syntaxhighlighter  bash" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2">`npm ``install` `-g forever`</div>
<div class="line number2 index1 alt1">`forever start index.js`</div>
</div></td>
</tr>
</tbody>
</table>
2\. 用自带的服务nohub
<table class="syntaxhighlighter  js" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2">`nohup node index.js &gt; myLog.log 2&gt;&amp;1 &amp;`</div>
</div></td>
</tr>
</tbody>
</table>