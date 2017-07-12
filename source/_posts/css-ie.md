---
title: 如何解决IE兼容性问题
id: 31
categories:
  - 技术点滴
date: 2013-05-28 12:42:16
tags:
---

摘要：所谓的浏览器兼容性问题，是指因为不同的浏览器对同一段代码有不同的解析，造成页面显示效果不统一的情况，在大多数情况下，我们的需求是，无论用户用什么浏览器来查看我们的网站或者登陆我...

所谓的浏览器兼容性问题，是指因为不同的浏览器对同一段代码有不同的解析，造成页面显示效果不统一的情况，在大多数情况下，我们的需求是，无论用户用什么浏览器来查看我们的网站或者登陆我们的系统，都应该是统一的显示效果。随着浏览器版本的增多，解决IE浏览器兼容性显得尤为重要.

一、`!important` (功能有限)

随着IE7对!important的支持, !important 方法现在只针对IE6的兼容.(注意写法.记得该声明位置需要提前.)

例如:
``` css
#example {
  width: 100px !important; /* IE7+FF */
  width: 200px; /* IE6 */
}
```

二、`CSS HACK`的方法(新手可以看看，高手就当路过吧)

首先需要知道的是：

所有浏览器 通用 `height: 100px;`

`IE6` 专用 `_height: 100px;`

`IE7` 专用 `*+height: 100px;`

`IE6、IE7` 共用 `*height: 100px;`

`IE7、FF` 共用 `height: 100px !important;`
<!--more-->
例如：
``` css
#example { height:100px; } /* FF */

* html #example { height:200px; } /* IE6 */

*+html #example { height:300px; } /* IE7 */
```
下面的这种方法比较简单

举几个例子：

1、IE6 - IE7+FF
``` css
#example {
  height:100px; /* FF+IE7 */
  _height:200px; /* IE6 */
}
```
其实这个用上面说的第一种方法也可以
``` css
#example {
  height:100px !important; /* FF+IE7 */
  height:200px; /* IE6 */
}
```
2、IE6+IE7 - FF
``` css
#example {
  height:100px; /* FF */
  *height:200px; /* IE6+IE7 */
}
```
3、IE6+FF - IE7
``` css
#example {
  height:100px; /* IE6+FF */
  *+height:200px; /* IE7 */
}
```
4、IE6 IE7 FF 各不相同
``` css
#example {
  height:100px; /* FF */
  _height:200px; /* IE6 */
  *+height:300px; /* IE7 */
}
```
或：
``` css
#example {
  height:100px; /* FF */
  *height:300px; /* IE7 */
  _height:200px; /* IE6 */
}
```
需要注意的是，代码的顺序一定不能颠倒了，要不又前功尽弃了。因为浏览器在解释程序的时候，如果重名的话，会用后面的覆盖前面的，就象给变量赋值一个道理，所以我们把通用的放前面，越专用的越放后面

解释一下4的代码：

读代码的时候，第一行`height:100px;` 大家都通用，IE6 IE7 FF 都显示`100px`

到了第二行`*height:300px;` FF不认识这个属性，IE6 IE7都认，所以FF还显示`100px`，而IE6 IE7把第一行得到的`height`属性给覆盖了，都显示`300px`

到了第三行`_height:200px;`只有IE6认识，所以IE6就又覆盖了在第二行得到的`height`,最终显示`200px`

这样，三个浏览器都有自己的`height`属性了，各玩各的去吧

这样说要是你还不明白，要么你去撞墙，要么我去!不过还是你去比较好。

哦，差点忘了说了：

`*+html` 对IE7的兼容 必须保证HTML顶部有如下声明：
``` html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"　"http://www.w3.org/TR/html4/loose.dtd">
```

三、使用IE专用的条件注释
``` html
<!--其他浏览器 -->
<link rel="stylesheet" type="text/css" href="css.css" />
<!--[if IE 7]>
<!-- 适合于IE7 -->
<link rel="stylesheet" type="text/css" href="ie7.css" />
<![endif]-->
<!--[if lte IE 6]>
<!-- 适合于IE6及以下 -->
<link rel="stylesheet" type="text/css" href="ie.css" />
<![endif]-->
```
貌似要编三套css，我还没用过，先粘过来再说

IE的if条件Hack
``` html
1\. <!--[if !IE]><!--> 除IE外都可识别 <!--<![endif]-->

2\. <!--[if IE]> 所有的IE可识别 <![endif]-->

3\. <!--[if IE 5.0]> 只有IE5.0可以识别 <![endif]-->

4\. <!--[if IE 5]> 仅IE5.0与IE5.5可以识别 <![endif]-->

5\. <!--[if gt IE 5.0]> IE5.0以及IE5.0以上版本都可以识别 <![endif]-->

6\. <!--[if IE 6]> 仅IE6可识别 <![endif]-->

7\. <!--[if lt IE 6]> IE6以及IE6以下版本可识别 <![endif]-->

8\. <!--[if gte IE 6]> IE6以及IE6以上版本可识别 <![endif]-->

9\. <!--[if IE 7]> 仅IE7可识别 <![endif]-->

10\. <!--[if lt IE 7]> IE7以及IE7以下版本可识别 <![endif]-->

11\. <!--[if gte IE 7]> IE7以及IE7以上版本可识别 <![endif]-->
```
注:
gt = Great Then 大于 > = > 大于号

lt = Less Then 小于 < = < 小于号

gte = Great Then or Equal 大于或等于

lte = Less Then or Equal 小于或等于

四、`css filter`的办法(据作者称是从国外某经典网站翻译过来的说)

新建一个css样式如下：
``` css
#item {
  width: 200px;
  height: 200px;
  background: red;
}
```
新建一个`div`,并使用前面定义的`css`的样式：
``` html
<div >some text here</div>
```
在`body`表现这里加入`lang`属性,中文为`zh`：
```html
<body lang="en">
```
现在对`div`元素再定义一个样式：
``` css
*:lang(en) #item{
  background:green !important;
}
```
这样做是为了用`!important`覆盖原来的css样式,由于`:lang`选择器ie7.0并不支持,所以对这句话不会有任何作用,于是也达到了ie6.0下同样的效果,但是很不幸地的是,safari同样不支持此属性,所以需要加入以下css样式：
```css
#item:empty {
  background: green !important
}
```
`:empty`选择器为`css3`的规范,尽管safari并不支持此规范,但是还是会选择此元素,不管是否此元素存在,现在绿色会现在在除ie各版本以外的浏览器上。


五、`FLOAT`闭合（clearing float）

网页在某些浏览器上显示错位很多时候都是因为使用了float浮动而没有真正闭合，这也是div无法自适应高度的一个原因。如果父div没有设float而其子div却设了float的话,父div无法包住整个子DIV，这种情况一般出现在一个父DIV下包含多个子DIV。解决办法：

１、给父DIV也设上float(不要骂我，我知道是废话)

2、在所有子DIV后新加一个空DIV(不推荐，有些浏览器可以看见空DIV产生的空隙)

比如：
```css
.parent{
  width:100px;
}

.son1{
  float:left;
  width:20px;
}

.son2{
  float:left;
  width:80px;
}

.clear{
  clear:both;
  margin:0;
  parding0;
  height:0px;
  font-size:0px;
}
``` html
<div class="parent">
  <div class="son1"></div>
  <div class="son2"></div>
</div>
```
3、万能 float 闭合

将以下代码加入Global CSS 中,给需要闭合的div加上 class=”clearfix” 即可,屡试不爽.

代码:
``` html
<style>
/* Clear Fix */
.clearfix:after {
  content:".";
  display:block;
  height:0;
  clear:both;
  visibility:hidden;
}

.clearfix {
  display:inline-block;
}
/* Hide from IE Mac \*/
.clearfix {display:block;}
/* End hide from IE Mac */
/* end of clearfix */
</style>
```
`:after`（伪对象）,设置在对象后发生的内容，通常和`content`配合使用，IE不支持此伪对象，非Ie 浏览器支持，所以并不影响到IE/WIN浏览器。这种的最麻烦。

4、`overflow:auto`（刚看到的，极力推荐）

只要在父DIV的CSS中加上`overflow:auto`就搞定。

举例：
``` css
.parent{
  width:100px;
  overflow:auto
}

.son1{
  float:left;
  width:20px;
}

.son2{
  float:left;
  width:80px;
}
```html
<div class="parent">
  <div class="son1"></div>
  <div class="son2"></div>
</div>
```
作者原话：原理是，外围元素之所以不能很好的延伸，问题出在了`overflow`上，因为`overflow`不可见（见W3C的解释）。现在只要将给外围元素添 加一个“`overflow:auto`”，就可以解决问题，结果是除了IE，真的可以解决。下来就要解决IE的问题了，再加上“`_height:1%`”，这个问题就完全解决了。

我试了一下，其实不加"`_height:1%`“在IE下也行，留着吧。

六、需要注意的一些兼容细节

1, FF下给 div 设置 padding 后会导致 width 和 height 增加(DIV的实际宽度=DIV宽+Padding), 但IE不会.

解决办法：给DIV设定IE、FF两个宽度，在IE的宽度前加上IE特有标记" * "号。

2, 页面居中问题.
```css
body {
  TEXT-ALIGN: center;
} 
```
在IE下足够了，但FF下失效。

解决办法：加上"`MARGIN-RIGHT: auto; MARGIN-LEFT: auto; `"

3, 有的时候在IE6上看见一些奇怪的间隙，可我们高度明明设好了呀。

解决办法：试试在有空隙的DIV上加上"`font-size:0px;`"

4, 关于手形光标. `cursor: pointer`. 而hand 只适用于 IE.

5, 浮动IE6产生的双倍距离
```css
#box{
  float:left;
  width:100px;
  margin:0 0 0 100px;
}
```
这种情况之下IE6会产生`200px`的距离

解决办法：加上`display:inline`，使浮动忽略

这里细说一下`block`,`inline`两个元素,Block元素的特点是:总是在新行上开始,高度,宽度,行高,边距都可以控制(块元素);Inline元素的特点是:和其他元素在同一行上,…不可控制(内嵌元素);
```css
#box{
  display:block; //可以为内嵌元素模拟为块元素 
  display:inline; //实现同一行排列的的效果
}
```
6 页面的最小宽度

min-width是个非常方便的CSS命令，它可以指定元素最小也不能小于某个宽度，这样就能保证排版一直正确。但IE不认得min-这个定义，但实际上它把正常的width和height当作有min的情况来使。这样问题就大了，如果只用宽度和高度，正常的浏览器里 这两个值就不会变，如果只用min-width和min-height的话，IE下面根本等于没有设置宽度和高度。比如要设置背景图片，这个宽度是比较重 要的。

解决办法：为了让这一命令在IE上也能用，可以把一个`<div>`放到 `<body>` 标签下，然后为div指定一个类：

然后CSS这样设计：
```css
#container{
  min-width: 600px;
  width:e-xpression(document.body.clientWidth < 600? “600px”: “auto” );
}

第一个min-width是正常的；但第2行的width使用了Javascript，这只有IE才认得，这也会让你的HTML文档不太正规。它实际上通过Javascript的判断来实现最小宽度。

7、UL和FORM标签的padding与margin

ul标签在FF中默认是有padding值的,而在IE中只有margin默认有值。FORM标签在IE中,将会自动margin一些边距,而在FF中margin则是0；

解决办法：css中首先都使用这样的样式ul,
```css
form{
  margin:0;
  padding:0;
}
```
给定义死了,后面就不会为这个头疼了.

8 ,DIV浮动IE文本产生3象素的bug

下面这段是我在网上粘过来的

左边对象浮动，右边采用外补丁的左边距来定位，右边对象内的文本会离左边有3px的间距.
```css
#box{
  float:left;
  width:800px;}
#left{
  float:left;
  width:50%;}
#right{
  width:50%;
}
*html #left{
  margin-right:-3px;
}
```
HTML代码
```html
<div id=box>
  <div id=left></div>
  <div id=right></div>
</div>
```
针对上面这段代码，下面说一下我的理解：

第一、只要right定义了width属性，在FF下绝对就会两行显示

第二、两个width都定义为百分比的话，就算都为100%在IE下也会一行显示。所以上面那句所谓“*html #left”根本没用，不加也在一行，除非你width定义的是数值才用得上。

所以说上面这段代码其实用处不大，至少在FF下不行。其实只要只定义left的width就行了，right不定义width就不管在IE还是FF下都能成功，但这样的话父DIV BOX并没有真正的包含LEFT和RIGHT两子DIV,可以用我上面说的第5种办法解决。最简单的办法就是在RIGHT中加上float:left就OK了，真磨叽！


9,截字省略号
```css
.hh {
  -o-text-overflow:ellipsis;
  text-overflow:ellipsis;
  white-space:
  nowrapoverflow:hidden;
}
```
这个是在越出长度后会自行的截掉多出部分的文字，并以省略号结尾。技术是好技术，很多人都喜欢乱用，但注意Firefox并不支持