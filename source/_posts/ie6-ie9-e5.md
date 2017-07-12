---
title: IE6-IE9兼容性问题列表及解决办法
id: 33
categories:
  - 技术点滴
date: 2013-05-28 12:46:06
tags:
---

# 第一章：HTML

## 第一节：IE7-IE8更新

### 1.如果缺少结束标记的 P 元素后跟 TABLE、FORM、NOFRAMES 或 NOSCRIPT 元素，会自动添加结束标记。

MSDN原文：Unclosed P elements are now automatically closed when followed by TABLE, FORM, NOFRAMES, or NOSCRIPT elements.

| 所属分类 | 版本更新 |
| ------ |-------|
| HTML   | IE7-IE8 |


**具体描述及示例**：

如果缺少结束标记的 P 元素后跟 TABLE、FORM、NOFRAMES 或 NOSCRIPT 元素，会自动添加结束标记，即：TABLE、FORM、NOFRAMES 或 NOSCRIPT 元素不能再嵌套在段落元素P中了b。

<!--more-->
考虑如下代码：
```html
<html>
   <head>
      <title>Simple P Element Closing Example</title>
      <meta http-equiv="X-UA-Compatible" content="IE8"/>
   </head>
   <body>
      <p>This is the first paragraph</p>
      <p style="margin-left:30px">This is another paragraph. <!-- P not closed -->
      <table border="1px" cellpadding="2px">
         <tr><td>This is a table cell.</td></tr>
      </table>
      <p>This is a third paragraph.</p>
   </body>
</html>
```
在这个例子中，第二个P元素没有关闭。在IE6, IE7下，Table元素显示为第二个p元素的子元素。第二个p元素是窗口的左边界缩进30像素。因为该表是一个P元素的子元素，它也从窗口的左边界缩进。IE7下Html结构图如下：

然而，与IE8时，在默认模式下，TABLE元素对齐到左边缘。因为IE8会自动关闭显示表元素之前闭合的P元素，TABLE元素的子元素。IE8下Html结构图如下：

**解决方案及正确写法**:

请注意此特性， 在代码中规避风险。

### 2.支持格式正确的有效标记，不再支持格式错误的 HTML。

MSDN原文：Malformed HTML is no longer supported, in favor of well-formed, valid markup.

| 所属分类 | 版本更新 |
| ------ |-------|
| HTML   | IE7-IE8 |

**具体描述及示例**：

支持格式正确的有效标记，不再支持格式错误的 HTML

Malformed HTML is no longer supported, in favor of well-formed, valid markup.

Parser error correction for malformed HTML has changed in IE8 Standards Mode. Pages depending on the way IE7 performs error correction may encounter issues as a result.
```html
<ul>
  <li>1.1
    <ul>
      <li>1.1.1</li>
      </li> <!—多了一个标记，will Closes 1.1 in IE8, but not IE7 -->
      <li>1.1.2</li>
    </ul>
  </li>
</ul>
```

在IE8下可以看到如下效果图：

**解决方案及正确写法**:

HTML标记写法要严谨。

Ensure your markup is well-formed and valid.
```html
<ul>
  <li>1.1
    <ul>
      <li>1.1.1</li>
      <!-- </li> -->
      <li>1.1.2</li>
    </ul>
  </li>
</ul>
```
修改后，IE8下效果图如下：

## 第二节: IE8-IE9更新

1.  表对象模式现在更加符合其他浏览器。

MSDN原文：Table Object Model Is Now More Consistent with Other Browsers.

| 所属分类 | 版本更新 |
| ------ |-------|
| HTML   | IE8-IE9 |

**具体描述及示例**：

为了提高IE和其他浏览器之间的一致性，IE9的标准模式的表Table发生了以下变化：


•额外的THead和TFoot元素不会出现在table.tBodies集合中。这里所指的是table.tBodies属性，并不是在tBody里面放thead或者tfoot。如果有多余的thead或者tfoot，IE9模式下不会把它们计入在内，而在IE8模式下会把多余的thead或tfoot单独计入到一个tBody。

•Table的行集合有着不同的顺序。无论他们在文档内的顺序是什么，首先是THead内容, 其次是TBody内容，最后才是TFoot内容。

•调用rows统计将返回一个表内的所有层次的TR行数，包括直属TR行。也就是指把table里面的所有TR对象都计入在rows列表里面，而不论它是在根节点还是thead/tfoot/tbody里面

•使用getElementsByTagName和HtmlElement.children方法不返回注释节点。


To improve consistency between Windows Internet Explorer and other browsers, the IE9 Standards mode includes the following changes to the table object model:


•Extra thead and tfoot elements do not appear in the tBodies collection.

•The rows collection has a different ordering. First, it includes any rows in the thead element, then all remaining rows that are not in the tfoot element, and then any rows in the tfoot element, regardless of their order in the document.

•A call for rows returns all rows at all depths within the table, including direct row children of the table.

•The getElementsByTagName and HtmlElement.children methods do not return comment nodes.

如果不考虑这些变化在您的应用程序，应用程序可能会遇到次要的脚本错误，页面始终保持在加载中状态，或创建非预期内容等错误。


If you do not consider these changes in your application, the application might encounter script errors that are minor, that keep pages from loading, or that create content that is not intended.


考虑如下代码：
```html
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<title>Table Test</title>
<script type="text/javascript">
  function CheckTable() {
    var table = document.getElementById("mytable");
    var tBodyResult = "有" + table.tBodies.length + "个tBody\n\n";
    var rows = table.rows;
    var length = rows.length;
    var rowResult = "";
    for (var i = 0; i < length; i++) {
      rowResult +=  "第" + (i+1) + "行: "+ rows[i].innerHTML + "\n";
    }
    alert(tBodyResult + rowResult);
  }
</script>
</head>
<body>
  <table id="mytable" >
    <tbody>
      <tr><td>Row1</td></tr>
      <tr><td>Row2</td></tr>
    </tbody>
    <tfoot>
      <tr><td>Foot</td></tr>
    </tfoot>
    <thead>
      <tr><td>Head1</td></tr>
    </thead>
    <thead>
      <tr><td>Head2</td></tr>
    </thead>
  </table>
  <button onclick="CheckTable()">Click Me</button>
</body>
</html>
```
点击Click Me按钮，IE8将弹出提示，显示有三个tBody, 实际上只有一个tbody，但是IE8将多余的一个thead和一个tfoot都各自单独计入一个tBody里了。而IE9则不会有这个问题，只显示有一个tBody。


注释：Table包含 thead、tfoot 以及 tbody 子元素， thead元素用于定义表格的表头，tfoot用于定义表格的表尾，tbody为表格中的主体内容。thead 元素应该与 tbody和 tfoot元素结合起来使用。


**解决方案及正确写法**:

注意此特性，规避风险。

**相关资源：**

1.  文本布局使用自然度量而不是图形设备接口 (GDI) 度量。

MSDN原文：Text Layout Uses Natural Metrics.

| 所属分类 | 版本更新 |
| ------ |-------|
| HTML   | IE8-IE9 |

**具体描述及示例**：

对于IE9 标准模式中的文字版面配置， IE9 使用自然度量法，而非旧版IE 浏览器使用的图形装置介面(GDI) 度量法。GDI 度量法会将文字与像素界限对齐，而自然度量法则会使用内部像素的间距来呈现更准确且更容易阅读的文字。


IE 的其他文件模式则继续使用GDI 度量法。


针对旧版文件模式所写的页面版面配置在IE9 中可能无法正确​​显示。 最常見的错误是未预期的文字換行，這可能会遮盖位于换行文字下方的元素。当文字方块沒有多余的水平空间或是当文字方块的大小与頁面上的其他元素(例如图形) 相连时，很可能会发生此错误。


For text layout in IE9 Standards mode, Windows Internet Explorer 9 uses natural metrics instead of the graphical device interface (GDI) metrics that other Microsoft Windows browsers use. GDI metrics align text to pixel boundaries, but natural metrics use inter-pixel spacing to create more accurately rendered and readable text.


Other document modes for Windows Internet Explorer continue to use GDI metrics.


Page layouts that are written for earlier document modes might display incorrectly in Internet Explorer 9\. The most common error is unexpected text wrapping, which can cover elements that are below the wrapped text. This error is most likely when a text box does not include extra horizontal space or when the size of the text box is connected to another element on the page, such as a graphic.


**解决方案及正确写法**:

不要假设特定字型的大小在不同浏览器间或是在相同浏览器内都会依相容的方式呈现，因为使用者有可能会缩放浏览器的字体 (例如，125%)。


利用下列设计准则，可确定您的网页显示文字版面配置的方式一致：

1.将Textbox的大小设定为特定的像素数。

2.在Textbox中留些余量空间，避免空间过紧。

3.使用非Static大小的Textbox。

4.在与其他页面元素相连接处包含额外空间。

5.如果您允许用户改变页面字体的大小，就要请您确认页面可以适应文字换行。

6.如果您发现文字换行的问题，请您调整页面，确保页面可以恰当地呈现文字。


避免对文字版面配置采用下列设计：

1.指望字型大小在不同浏览器间以相同的方式呈现。

2.使用Static大小的textbox。


关于Static大小的textbox, 含义如下例：
```html
<input type="text" style="width:100px" />
```
这里就指定了text box的size，非静态non-statically意思是说我们不要去指定size。如果一定要指定，尽量用如上的px作为单位。

# 第二章：CSS

## 第一节：IE6-IE7更新

### 1\. 方框模型溢出内容现与方框相交，不再让方框自动增长适应内容。

MSDN原文：Box model overflow content now intersects box, no longer auto-grows box div to fit content.

| 所属分类 | 版本更新 |
| ------ |-------|
| CSS   | IE6-IE7 |

**具体描述及示例**：

在IE7中，为了适应CSS2.1 box model，微软修改了溢出的行为。溢出是用来描述当一个块元素的内容溢出它的区域时，这些内容是否被剪切掉的一种方法。默认情况下，内容不被剪切，也就是，它有可能呈现在区域以外。过去的IE不支持这个行为。内容总是需要适合区域的大小。想象一个宽和高都是100px的区域，如果内容小于100px，那么没有问题。如果内容超过了尺寸，我们需要自动增长区域大小来适合内容。


请看下面的代码示例。
```html
<style type="text/css">
div {
  width: 100px;
  height: 100px;
  border: thin solid red;
}

blockquote {
  width: 125px;
  height: 100px;
  margin-top: 50px;
  margin-left: 50px;
  border: thin dashed black
}

cite {
  display: block;
  text-align: right;
  border: none
}

p {
  margin: 0;
}
</style>
<div>
  <blockquote>
    <p>some text long enough to make it interesting.</p>
    <cite>- anonymous</cite>
  </blockquote>
</div>
```

下图显示了在IE6和IE7中此段代码的不同效果，(上图为IE6，下图为IE7)：



从图中可以看出，`<blockquote>`内容超出了div的边界，在IE7下，div不再自动增长来装下`<blockquote>`，而是让`<blockquote>`伸出div的边界。


As you can see, we now honor the height and width settings of a box. In this example, the `<blockquote>` content now renders outside of the boundaries of the parent `<div>` (box with red borders).


**解决方案及正确写法**:

请检查相关的页面设计，如果以前依赖自动增长这一特性，现在就需要修改代码，来避免页面布局显示错误的风险。

If your layout relied on us "growing" the box (if your content did not fit the dimensions you gave it) then this can lead to breaks. You can easily discover breaks related to overflow by observing content suddenly overlapping other content.

**相关资源：**

[http://msdn.microsoft.com/en-us/library/bb250496(VS.85).aspx](http://msdn.microsoft.com/en-us/library/bb250496(VS.85).aspx)

### 2\. 不再支持某些 CSS 筛选器（如 *HTML、_underscore 和 /**/ 注释）。

MSDN原文：Certain CSS filters (for example, *HTML, _underscore, and /**/ comment) are no longer supported.）

| 所属分类 | 版本更新 |
| ------ |-------|
| CSS   | IE6-IE7 |

**具体描述及示例**：

虽然CSS标准已经存在，但是并不保证所有的浏览器用同样的方式呈现页面。这些标准可能含有未经定义的部分，并不是所有的组件等会被所有的浏览器去执行，并且已知的执行也可能存在问题。CSS标准并不提供一个方法去指定一个特定的浏览器版本，所以网络开发者社区开发了CSS filter（也被称作”CSS hacks”）。这些filter利用浏览器的问题或者未执行的特性来隐藏针对特殊浏览器的CSS样式规则。当我们修复了这些问题并且改进了CSS支持后，一些CSS filter将不再可用。


如果你使用这些filters，你应该了解它们的效果。这个可以帮助你做出针对以后版本的Internet Explorer和其它浏览器的更有效的并且适应改进后的CSS的设计。


在IE7中，我们修改了许多潜在解析错误，这些错误有可能会阻止下面的filter在以前的IE版本中正常工作。如果你的页面中包含这些filter，请去除或者更换它们。


Even though standards like CSS are available it is not a guarantee that all browsers render the same way. The standard might have undefined parts, not all components are equally implemented by all browser vendors, and existing implementations might have bugs. The CSS standard does not provide a way to target specific browser versions and as a result the Web developer community has developed CSS filters (also called "CSS hacks"). These filters take advantage of browser bugs or unimplemented features to hide CSS style rules from specific browsers. As we fix these bugs and improve CSS support, some CSS filters will stop working.


If you use such filters, you should understand their effects. In this long run, this will help you create designs that more effectively adapt to improved CSS support in later versions of Internet Explorer and other browsers.


In IE7, we fixed many underlying parser bugs that prevented the following filters from working in earlier versions of IE. If your page contains these filters, please remove or replace them (at the end of this article we will offer other means of targeting reliable IE versions).


具体举例如下：


*HTML filter

这个CSS filter基于一个解析错误。它被用于显示排除内容。这些内容将被Internet Explorer7和以后的版本忽略。
```css
/* The following rules used to apply only to
IE but now get ignored by IE7 and higher */
* html{

}

* html body{

}

* html .foo{

}
```

下划线filter

这个CSS filter基于一个解析错误。它被用来显示被排除的属性。这个内容现在被Internet Explorer以及之后的版本认为是一个自定义属性。自定义属性意味着它仍然可以使用，但是并不默认就拥有一个值。

```css
/* The following rule used to apply min-height
to browser who understand this property and
height to IE.  In IE7, _height will be treated
as a custom property (no height will be applied) */

.myclass {
  min-height: 300px;
  _height: 300px;
  ...
}
```

/**/注释filter

这个CSS filter基于一个解析错误。它被用来在strict模式下隐藏属性（这个filter在quirk模式下不起作用）。在Internet Explorer7中，这个属性可以被解析和使用。

```css
/* The following rule used to hide the height
property to Internet Explorer. In IE7, the
value will be applied */

.myclass {
  height/**/: 300px;
  ...
}
```
**解决方案及正确写法**:

如果使用了上述CSS筛选器，停用即可。

**相关资源：**

### 3\. 已解决SELECT 元素不能被div覆盖的问题。

MSDN原文：Applications that relied on SELECT element to get an HWND to use with Microsoft Win32 APIs may break because SELECT element is now a windowless control.

| 所属分类 | 版本更新 |
| ------ |-------|
| CSS   | IE6-IE7 |

**具体描述及示例**：

Select元素现在已改为无窗口控件， 这项改变使z-order 和 zoom缩放能正常工作了，也就说， 这次彻底解决了IE6中Select不能被div覆盖的问题，除此之外，其他都和以前保持一致；

但是，如果有应用程序依赖从Select控件获取窗口的HWND就必须修改为使用DOM了，因为Select元素现在是无窗口控件，也就得不到HWND了。

Select Element—The Select control is now a windowless control. This change enables z-order and zoom to work correctly. The HTML and Document Object Model (DOM) for Select remain the same, so Web developers and end users interact with Select as before; however, applications that relied on getting an HWND from Select to use with Microsoft Win32 APIs must be modified to use the DOM.
** **

**解决方案及正确写法**:

注意此特性即可。

**相关资源：**

Select control: CSS style-able and not always on top

[http://blogs.msdn.com/b/ie/archive/2006/01/17/514076.aspx](http://blogs.msdn.com/b/ie/archive/2006/01/17/514076.aspx)


## 第二节：IE7-IE8更新

### 1.不再支持 CSS 表达式，改为支持增强的 CSS 或 DHTML 逻辑。

MSDN原文：CSS expressions no longer supported, in favor of improved CSS support or DHTML logic.

| 所属分类 | 版本更新 |
| ------ |-------|
| CSS   | IE7-IE8 |

**具体描述及示例**：
不再支持 CSS 表达式，改为支持增强的 CSS 或 DHTML 逻辑。
CSS expressions no longer supported, in favor of improved CSS support or DHTML logic.

Support for CSS Expressions has been removed in IE8 Standards Mode.

CSS 表达式的例子：
```css
#main {
  background-color: expression(
    (new Date()).getHours()%2 ? "#000" : "#fff"

  );
}
```
**解决方案及正确写法**:

停用CSS Expressions，重构代码.
Refactor to utilize either improved CSS support or DHTML logic.
```javascript
/* Script */
var elm = document.getElementById("main");
if((new Date()).getHours()%2) {
elm.style.backgroundColor = "#000";
} else {
elm.style.backgroundColor = "#fff";
}
```
**相关资源：**

### 2.currentStyle 对象的Unset属性现在返回其初始值。

MSDN原文：Unset properties on the currentStyle object now return their initial value. This is the root cause of issues with the ASP.NET menu control.

| 所属分类 | 版本更新 |
| ------ |-------|
| CSS   | IE7-IE8 |

**具体描述及示例**：

现在，currentStyle 对象的Unset 属性会返回其初始值”auto”， 这是 ASP.NET 菜单控件问题的根本原因。

Unset properties on the currentStyle object now return their initial value. This is the root cause of issues with the ASP.NET menu control (example).

Initial CSS Property Values

Unset properties on the currentStyle object now return their initial value. Relying on the old initial values for CSS properties such as z-index can cause problems. This is the root cause of issues with the ASP.NET menu control.
```javascript
var zIndex = elm.currentStyle.zIndex;
if(zIndex == 0) {
  // custom code
}
```

注释1：currentStyle 对象，它用来返回元素上的样式表，但是 style 对象只返回通过 STYLE 标签属性应用到元素的内嵌样式。因此，通过 currentStyle 对象获取的样式值可能与通过 style 对象获取的样式值不同。例如，如果段落的 color 属性值通过链接或嵌入样式表设置为红色( red )，而不是内嵌的话，对象.currentStyle.color 将返回正确的颜色，而对象 style.color 不能返回值。但是，如果用户指定了 <P STYLE="color:'red'">，currentStyle 和 STYLE 对象都将返回值 red。

注释2：z-index 属性设置元素的堆叠顺序。拥有更高堆叠顺序的元素总是会处于堆叠顺序较低的元素的前面。

**解决方案及正确写法**:

例子：下面button不设置style属性(也就谈不上置width和zindex了)，则currentStyle.width和currentStyle.zIndex都返回默认值auto.
```html
<input type="button" id="btnAdd" value="123"  />
alert(window.document.getElementById("btnAdd").currentStyle.width);
alert(window.document.getElementById("btnAdd").currentStyle.zIndex);
```
要做如下双重检查：
Perform a check for both the backwards compatible value and the standardized initial value.
```javascript
var zIndex = elm.currentStyle.zIndex;
if(zIndex == 0 || zIndex == "auto") {
  // custom code  
}
**相关资源：**

currentStyle 对象

[http://www.phpx.com/man/dhtmlcn/objects/currentStyle.html](http://www.phpx.com/man/dhtmlcn/objects/currentStyle.html)

### 3.style 对象的 Unset 属性值现在返回空字符串。

MSDN原文：Unset properties values on the style object now return empty string.

| 所属分类 | 版本更新 |
| ------ |-------|
| CSS   | IE7-IE8 |

**具体描述及示例**：
style 对象的 Unset 属性值现在返回空字符串。
Unset properties values on the style object now return empty string.
Unspecified CSS Property Values
Unset properties on the style object now return the empty string for improved compliance with the DOM Level 2 Style Specification. Expecting CSS properties such as z-index to have a value when they have not been explicitly set can lead to problems.
```javascript
var zIndex = elm.style.zIndex;
if(zIndex === 0) {
  // custom code
}
```
**解决方案及正确写法**:
要做如下双重检查：
Perform a check for both the backwards compatible value and the empty string.
```javascript
var zIndex = elm.style.zIndex;
if(zIndex === 0 || zIndex === "") {
  // custom code
}
```
**相关资源：**

## 第三节：IE8-IE9更新

1.  泰语和东亚语文本和字体大小的显示可能小于其他字样。

MSDN原文：Thai and East Asian Text and Font Sizing.

| 所属分类 | 版本更新 |
| ------ |-------|
| CSS   | IE8-IE9 |

**具体描述及示例**：

泰文和东亚文字在IE9 看起来可能比在IE8 和旧版本中还要小。
在IE8 中，泰文和東亚文字可在下列情況下以大于指定大小的字型呈现：
1.指定font size为9pt 或更小.

2.指定的font-family不支持泰文或東亚文字，例如Arial指定的字型系列不支持泰文或东亚文字。

因此，8pt Arial 的泰文段落会使用[Internet Options-Fonts] 中指定的后援字型来呈现，而后者之后会配合Ariel 公制放大调整。因此，如果网页作者要求使用8pt 的字型大小，则实际尺寸会更大。

在IE9 中，则始终采用指定的字型大小。因此，由于后援字型不会再放大调整，文字可能看起来可能会比较小。

Thai and East Asian text may look smaller in IE9 than in IE8 and earlier releases. In IE8, Thai and East Asian text could be rendered at a larger font size than specified when:

•The specified font size was 9pt or smaller

•The specified font family did not support Thai or East Asian characters such as Arial

Thus, a Thai paragraph of 8pt Arial would be rendered using the fallback font specified in Internet Options-Fonts and the latter would then be scaled up to match Arial metrics. As a result, the real size is larger than it would have been if the web author had requested that font at 8pt.

In IE9, the specified font size is always respected. Thus, as the fallback font is no longer scaled up, text may appear smaller.

**解决方案及正确写法**:

请尽可能确定CSS font-family 属性的第一个值支持您的语言。

此问题还会影响到多语言界面的CSS字体设置。

**相关资源：**

** **

### 2.某些行为连接方法在 XML 模式中不可用。

MSDN原文：Some Behavior-Connecting Methods Do Not Work in XML.

| 所属分类 | 版本更新 |
| ------ |-------|
| CSS   | IE8-IE9 |

**具体描述及示例**：

用于连接行为的标记式表单在Windows IE9 模式中可运作，但在xml 模式中则无法运作。

The markup-based form for connecting behaviors works in Windows Internet Explorer 9 modes, but it does not work in xml mode.

行为可在网页的最上层指定，如下面的程式码范例所示。

A behavior can be specified at the top of a webpage, as the following code example shows.
```html
<html xmlns:myNamespace>
<?import namespace="myNamespace" implementation = "my.htc">
…
<myNamespace:calendar/>
```

此问题只影响新内容。
This problem affects only new content.

**解决方案及正确写法**:

请不要使用HTML标记，而是通过CSS的behavior属性注册，如下例所示。

Instead of using HTML markup, use Cascading Style Sheets (CSS)-based registration through the behavior property, as the following code example shows.
```html
<style>
.calendar {
  ms-behavior: url(my.htc);
}
</style>
…
<div class="calendar"></div>
```

您可以透过class属性以外的元素使用CSS注册。
You can use CSS-based registration through elements other than the class attribute.

**相关资源：**

# 第三章：Javascript and DOM
## 第一节：IE6-IE7更新
### 1.不再允许用于绕过 window.close 提示的 window.opener 技巧。

MSDN原文：window.opener trick used to bypass the window.close prompt is no longer allowed.

| 所属分类 | 版本更新 |
| ------ |-------|
| Javascript and DOM   | IE6-IE7更新 |

**具体描述及示例**：

不再允许用于绕过 window.close 提示的 window.opener 技巧。

window.opener and window.close--Internet Explorer 7 no longer allows the window.opener trick to bypass the window.close prompt. Browser windows cannot close themselves unless the windows were created in script. This security enhancement no longer allows browsing to a random site when the main browser window closes unexpectedly.

**解决方案及正确写法**:

在IE6中，当我们用这种方法:`Response.Write("<script>window.close()</script>")`

总是会提示:你查看的网页试图关闭的提示。在IE6下, 可以用以下方法，去掉提示，直接关闭窗体: `Response.Write("<script>window.opener=null;window.close()</script>")`

opener只要设为任何值都可以,不会出现提示。

在IE7及以上版本中，此写法就不被允许了。

注意此新特性即可。

**相关资源：**

### 2.从脚本创建的模式或无模式对话框看起来似乎稍微变大。

MSDN原文：Modal or modeless dialogs created from script might seem slightly bigger.

| 所属分类 | 版本更新 |
| ------ |-------|
| 其他   | IE6-IE7更新 |

**具体描述及示例**：

从脚本创建的模式或无模式对话框看起来似乎稍微变大。

Changes that affect modal or modeless dialogs created from script--Modal or modeless dialogs created from script in Internet Explorer 7 might seem to be slightly bigger than their Internet Explorer 6 counterparts. This is caused by a change to the behavior of the dialogWidth and dialogHeight properties, which now set and retrieve dimensions of the content area of a dialog (from Internet Explorer 7 and later). It will no longer be necessary to calculate the area lost by elements of a dialog’s frame.

**解决方案及正确写法**:

需要目测，看是否需要调整。

**相关资源：**

Why Does IE Resize My Dialogs?

[http://blogs.msdn.com/b/ie/archive/2006/08/25/719355.aspx](http://blogs.msdn.com/b/ie/archive/2006/08/25/719355.aspx)

## 第二节：IE7-IE8更新

1.  支持“class”语法，不再支持“className”属性语法。
MSDN原文：“className” attribute syntax no longer supported, in favor of “class” syntax.

| 所属分类 | 版本更新 |
| ------ |-------|
| Javascript and DOM   | IE7-IE8 |

**具体描述及示例**：

在IE7中，必须使用属性名称为“ClassName”来设置和检索元素的类。在IE8中已经被修改为符合标准，因此需要使用“class”来设置和检索元素的类。

In IE7, "className" had to be used as the attribute name to set and retrieve the class of an element. This has been fixed for standards-compliance in IE8 Standards Mode. Using the old approach will create an attribute named "className" that has no affect on the actual class assigned to an element.
```javascript
return elm.getAttribute("className");
```

**解决方案及正确写法**:

使用规范的名称，“class”，而不是为“ClassName”。
```javascript
return elm.getAttribute("class");
```

例子：
```javascript
function GetClass() {
  alert(window.document.getElementById("btnAdd").getAttribute("className"));
  //alert(window.document.getElementById("btnAdd").getAttribute("class"));
}
```
```html
<body onload="GetClass()">
  <form id="form1" runat="server">
    <div>
      <input type="button" id="btnAdd" value="123"  />
    </div>
  </form>
</body>
```
使用`getAttribute("className")`，在IE8中会得到Null的错误提示。

使用`getAttribute("class")`，在IE8中会得到INPUT的正确值。

**相关资源：**

** **

### 2.属性集合不再包含 Internet Explorer 可识别的所有可能属性。

MSDN原文：**The attributes collection no longer contains all possible attributes recognized by Internet Explorer.**

| 所属分类 | 版本更新 |
| ------ |-------|
| Javascript and DOM   | IE7-IE8 |

**具体描述及示例**：

属性集合不再包含 Internet Explorer 可识别的所有可能属性.

The attributes collection no longer contains all possible attributes recognized by Internet Explorer.

The attributes collection no longer contains all possible attributes recognized by IE, but only those that have been explicitly set. Scripts can fail if they depend on natively supported attributes always being present as they were in IE7.


例如：
```javascript
var attr = elm.attributes["checked"];
// Potential script error in IE8
return attr.specified;
```

**解决方案及正确写法**:

要先检查确认属性存在，而后再使用。

Do not assume an attribute will be in the attributes collection. Check for existence first.

正确写法：
```javascript
var attr = elm.attributes["checked"];
if(attr) return attr.specified;
else return false;
```
**相关资源：**

### 3.属性排序已更改，影响了属性集、innerHTML 和 outerHTML。

MSDN原文：**Attribute ordering has changed, affecting attributes collection, innerHTML, and outerHTML.**

| 所属分类 | 版本更新 |
| ------ |-------|
| Javascript and DOM   | IE7-IE8 |

**具体描述及示例**：

属性排序已更改，影响了属性集、innerHTML 和 outerHTML。

Attribute ordering has changed, affecting attributes collection, innerHTML, and outerHTML.

The ordering of attributes has changed, affecting the attributes collection as well as the values of innerHTML and outerHTML. Pages depending on a specific attribute ordering may break.
```javascript
attr = elm.attributes[1]; // May differ in IE8
```

**解决方案及正确写法**:

请用属性取id代替属性所在位置索引。

Reference attributes by name as opposed to their position within the attributes collection.
```javascript
attr = elm.attributes["id"];
```
**相关资源：**


### 4.GetElementById 区分大小写，且不再搜索名称属性。

MSDN原文：GetElementById is case sensitive and no longer searches name attributes.

| 所属分类 | 版本更新 |
| ------ |-------|
| Javascript and DOM   | IE7-IE8 |

**具体描述及示例**：

GetElementById 区分大小写，不再搜索名称属性

GetElementById is case sensitive and no longer searches name attributes.

The method getElementById is now case-sensitive and no longer searches name attributes.
```html
<div id="Test"></div>
<script type="text/javascript">
  // No element is found because of case difference
  var test = document.getElementById("test");
</script>
```

**解决方案及正确写法**:

遵守区分大小写的特性即可。

Ensure case-correctness and use getElementsByName when searching name attributes.
```html
<div id="Test"></div>
<script type="text/javascript">
  // Element Test is found
  var test = document.getElementById("Test")
</script>
```
**相关资源：**


## 第三节：IE8-IE9更新

1.  createElement 方法中不允许使用尖括号<> 。
MSDN原文：Angle Brackets Are Not Allowed in the createElement Method.

| 所属分类 | 版本更新 |
| ------ |-------|
| Javascript and DOM   | IE8-IE9 |

**具体描述及示例**：

IE9不能识别createElement方法内的角括弧(< >)。如果您在IE9 的页面中使用角括弧，会发生异常。


Windows IE 9 does not recognize angle brackets (< >) within the createElement method. If you use angle brackets in IE 9 webpages that are not set to previous Windows IE Standards document modes, an exception occurs. For example, the following code example will cause an exception in IE9 mode.


例如，下面的例子在IE9 模式中引发异常。
```javascript
var el=document.createElement("<div id='myDiv'>");
```
**解决方案及正确写法**:

要顺应IE9 中的这项变更有两种方法：

1.  使用setAttribute API 建立​​元素并分别新增属性，如下面的代码范例所示。
```javascript
var elm = document.createElement("div");
elm.setAttribute("id","myDiv");
```javascript
1.  使用innerHTML API 在父元素内建立元素，如下面的代码范例所示。
```javascript
var parent=document.createElement("div");
parent.innerHTML="<div id='myDiv'></div>";
var elm=parent.firstChild;
```
或者，也可以将网页设定为IE9 以前的文件模式。

**相关资源：**


1.  IE9 标准模式不支持arguments.caller 属性。
MSDN原文：IE9 Standards Mode Does Not Support the arguments.caller Property.

| 所属分类 | 版本更新 |
| ------ |-------|
| Javascript and DOM   | IE8-IE9 |

**具体描述及示例**：

IE9 标准模式中不再支持arguments.caller 属性。

当建立argument objects时，在IE8 和Quirks 中的所有模式，以及IE9 的IE7 标准和IE8 标准模式，都会建立一个名为“caller” 的属性。此caller 属性会储存对呼叫它的函数的argument Object参照。

When argument objects are created, in all modes of IE8 and Quirks, IE7 Standards, and IE8 Standards modes of IE9, a property with the name “caller” is created. This caller property stores the reference to the argument object of the function that called it.

在下例中，IE8 和Quirks 中的所有文件模式，以及IE9 的IE7 标准和IE8 标准文件模式会传回“1”。 IE9 标准模式会发出「Object为null 或未经定义」的错误。

In the following example, all document modes in IE8 and Quirks, IE7 standards, and IE8 standards document modes in IE9 return “1”. IE9 standards mode issues the script error “object is null or undefined”.

```javascript
function alertCallerLength() {
  alert(arguments.caller.length);
}

function callingFunction() {
  alertCallerLength();
}

callingFunction(1);
```

**解决方案及正确写法**:

停用用此属性即可。

**相关资源：**


### 3.不再支持使用不带“.call”或“.bind”的函数指针调用方法。

MSDN原文：**Calling a Method with a Function Pointer without ".call" or ".bind"**

| 所属分类 | 版本更新 |
| ------ |-------|
| Javascript and DOM   | IE8-IE9 |

**具体描述及示例**：

旧版IE中，为了使JavaScript 代码更加精简，常见的做法是将常用的方法储存为变量，然后将该变量替代该方法来使用：
```javascript
var d = document.writeln;
d("<script language=VBScript>");
```
在IE9 中，需要有Object才能调用方法。window对象预设是在全域范围内提供(比如在上例中)。但是window对象并没有"writeln" 的方法，因此会报告JavaScript「call Object无效」的错误信息。

Previous versions of Internet Explorer supported caching a pointer to a method and then using the cached pointer to call the method. This support was removed in Internet Explorer 9 in order to increase interoperability with other browsers.

A common practice on webpages targeting older versions of IE was to save common methods to a variable and use that variable as a proxy for the method in order to make JavaScript code more compact:
```javascript
var d = document.writeln;
d("<script language=VBScript>");
```
In IE9, an object is required in order to invoke the method. By default the "window" object is provided in global scope (such as in the previous example). However, the "window" object does not have a method "writeln" and so the JavaScript error message "Invalid calling object" is reported.

**解决方案及正确写法**:

简单解决此问题的方法是使用"call" 方法(所有函数的属性) 明确提供适当的呼叫Object：

```javascript
d.call(document, "<script language=VBScript>");
```

长期解决此问题的方法是使用JavaScript 的"bind" API，建立隐含的呼叫Object与方法的关联。方法如下(同样是取自上例)：
```javascript
var d = document.writeln.bind(document);
d("<script language=VBScript>"); // Now this is OK
```
原文：
Now you must specify the target for the method call just as you do in all other browsers. So while this code works in IE8 and earlier:
```javascript
var d = document.writeln;
d("<script language=VBScript>");
```

Now it fails in IE9 just as it fails in all other browsers. An easy fix for this issue is to use the "call" method (a property of all functions) to explicitly provide the appropriate calling object:
```javascript
d.call(document, "<script language=VBScript>");
```

The long term fix for this is to use JavaScript's "bind" API to associate an implicit calling object with the method. This is done as follows (again, drawing on the previous example):
```javascript
var d = document.writeln.bind(document);
d("<script language=VBScript>"); // Now this is OK
```
**相关资源：**

1.  不再连接内容属性和 DOM expando。
MSDN原文：Content Attributes and DOM Expandos Are No Longer Connected.

| 所属分类 | 版本更新 |
| ------ |-------|
| Javascript and DOM   | IE8-IE9 |

**具体描述及示例**：


例如：
```html
<div id="myElement" myAttr="custom"></div>
```

在下面的范例中，'id' 和'className' 都是预先定义的属性：
```javascript
var div = document.getElementById("myElement");
var divId = div.id; // Gets the value of the id content attribute
var divClass = div.className; // Gets the value of the class content attribute
```

在IE8 和之前的版本中，'myAttr' 内容属性的存在等于暗示'myAttr' DOM expando 的存在：
```javascript
var divExpando = div.myAttr; // divExpando would get the value "custom" in IE8
```

IE9 中的突破性变更是使用者定义的内容属性将不再暗指DOM expando。
```javascript
var divExpando = div.myAttr; // divExpando would get an undefined value
```

In past versions of Internet Explorer, content attributes were represented on JavaScript objects as DOM expandos. In Internet Explorer 9, this link between content attributes and DOM expandos has been severed in order to increase interoperability between IE and other browsers.

A content attribute is an attribute that is specified in the HTML source, for example, <element attribute1="value" attribute2="value">. Many content attributes are predefined as part of HTML; HTML also supports additional user-defined content attributes.

DOM expandos are the values that can be retrieved from an object in JavaScript, for example, document.all["myelement"].domExpando. Most objects have a predefined set of properties that typically represent the value of their same-named content attribute. JavaScript supports additional user-defined properties as well.


**解决方案及正确写法**:

在JavaScript 代码中几乎无法直接在失败点看出此问题。而往往是在应用时才会看到失败的情形。

若要修正此问题，请使用'getAttribute'来获取使用者定义的内容属性值。建议对所有IE 版本采取此因应措施。例如(根据上个范例)：


不要使用：
```javascript
var divExpando = div.myAttr;
```

而改用：
```javascript
var divExpando = div.getAttribute("myAttr");
```
**相关资源：**

1.JavaScript 属性列举在IE9 中不同。

MSDN原文：JavaScript Property Enumeration Differs in Internet Explorer 9

| 所属分类 | 版本更新 |
| ------ |-------|
| Javascript and DOM   | IE8-IE9 |

**具体描述及示例**：

由于IE9 的JavaScript Object模型中所做的变更，JavaScript 属性的列举方式跟在IE8 中可能不太一样。

Due to the changes made in the JavaScript object model of Internet Explorer 9, JavaScript properties may be enumerated differently from how they are enumerated in Internet Explorer 8.


当在任何文件模式内使用for… 陈述式时，属性列举的顺序可能与IE8 传回的顺序不同。例如，数值属性现在是列举在非数值属性之前。下例说明IE8 与IE9 之间的列举顺序的差异：


When using the for…in Statement, in any document mode, the order of property enumerations may be different from the order returned by Internet Explorer 8\. For example, numeric properties now enumerate before non-numeric properties. The following sample illustrates the difference in enumeration order between Internet Explorer 8 and Internet Explorer 9:
```javascript
var obj = {first : "prop1", second: "prop2", 3: "prop3"};
var s = "";
for (var key in obj) {
  s += key + ": " + obj[key] + " ";
}
document.write (s);
```

在IE8 和IE9 中执行此范例会输出下列结果：

IE8：first: prop1 second: prop2 3: prop3

IE9：3: prop3 first: prop1 second: prop2

IE8 并不包含与原型物件的内建属性同名的属性列举。 IE9 中的所有文件模式則会在列举中包含這些属性。

Internet Explorer 8 does not include enumerations of properties that have the same name as built-in properties of a prototype object. All document modes in Internet Explorer 9 include these properties in the enumeration. The following sample illustrates this difference:

下列说明此项差异：
```javascript
var obj = { first: "prop1", toString : "Hello" }
var s = "";
for (var key in obj) {
  s += key + ": " + obj[key] + " ";
}
document.write (s);
```

在IE8 和IE9 中执行此范例会输出下列结果：

IE8的所有模式： first: prop1 first: prop1

IE9的所有模式： first: prop1 toString: Hello

**解决方案及正确写法**:

需要注意此项差异并修改受影响代码。

**相关资源：**

1.数学精确度在IE9 中不同。

MSDN原文：Math Precision Differs in Internet Explorer 9

| 所属分类 | 版本更新 |
| ------ |-------|
| Javascript and DOM   | IE8-IE9 |

**具体描述及示例**：

数学精确度在某些边界情况下与IE8 不同。

Math precision differs from Internet Explorer 8 in certain edge cases.

Chakra 引擎使用Streaming SIMD Extensions 2 (SSE2) (如果平台有支援的话)，达成更快速的数学运算，但是也会在精确度方面产生有别于IE8 的JScript 引擎的差异。 下列說明了這项差异：

The Chakra engine uses Streaming SIMD Extensions 2 (SSE2) if the platform supports them, which results in faster mathematical operations but also yields a difference in precision from the JScript engine of Internet Explorer 8\. The following example demonstrates the difference:
```javascript
function test() {
  var x = 6.28318530717958620000;
  var val = Math.sin(x);
  document.write(Math.abs(val))
}
test();
```
在IE8 的所有模式中，这会输出“2.4492127076447545e-16”。

当平台支持SSE2 时，在IE9 的所有模式中，这会输出“2.4492935982947064e-16”。

In all modes of Internet Explorer 8, this will print “2.4492127076447545e-16”. When the platform supports SSE2, in all modes of Internet Explorer 9, this will print “2.4492935982947064e-16”.

**解决方案及正确写法**:

需要注意此项差异。

**相关资源：**

1.间接“eval”函数调用的行为方式不同。

MSDN原文：Indirect 'eval' Function Calls Behave Differently in Internet Explorer 9.

| 所属分类 | 版本更新 |
| ------ |-------|
| Javascript and DOM   | IE8-IE9 |

**具体描述及示例**：

间接“eval”函数调用，会在IE9 中产生与IE8 不同的结果。

Calling eval methods indirectly (that is, other than by the explicit use of its name) inside a function produces different results in Internet Explorer 9 than it does in Internet Explorer 8.

在IE8 和Quirks 的所有模式中，以及IE9 的IE 7 标准和IE8 标准模式中，传递给间接eval 的字串是在本机函数范围内进行评估。在IE9 的IE9 标准模式中，则是根据ECMAScript 标准(第5 版) 在全域范围内进行评估。

In all modes of Internet Explorer 8 and Quirks, IE 7 standards, and IE8 standards modes of Internet Explorer 9, the string passed to the indirect eval is evaluated in the local function scope. In IE9 Standards mode of Internet Explorer 9, it is evaluated in the global scope as per the ECMAScript standard (5th edition).

在下例中，是通过将eval 函数赋值给变量并且把该变量当作eval 来调用的方式，间接调用eval 函数。

In the following example, the eval function has been called indirectly by assigning it to a variable and calling the variable as if it were eval.
```javascript
function test() {
  var dateFn = "Date(1971,3,8)";
  var myDate;
  var indirectEval = eval;
  indirectEval("myDate = new " + dateFn + ";");
  document.write(myDate);
}
test();
```

本范例在IE8 和Quirks 的所有模式中，以及IE9 的IE 7 标准和IE8 标准模式中将传回“Thu Apr 8 00:00:00 PDT 1971”。而在IE9 的IE9 标准模式中，这会输出“undefined”。

This sample will return “Thu Apr 8 00:00:00 PDT 1971” in all document modes of Internet Explorer 8 and Quirks, IE7 standards, and IE8 standards document modes in Internet Explorer 9\. However in IE9 standards mode of Internet Explorer 9, this would print “undefined”

**解决方案及正确写法**:

需要注意此项差异。

**相关资源：**


1.IE9 处理含大型索引的数组项目的方式不一样。

MSDN原文：Array elements with large indices are handled differently than in Internet Explorer 8.

| 所属分类 | 版本更新 |
| ------ |-------|
| Javascript and DOM   | IE8-IE9 |

**具体描述及示例**：

含大型索引的数组项目的处理方式与IE8 不同。

Array elements with large indices are handled differently than in Internet Explorer 8.

在数组的length 属性的初始值大于2E+31-1 (亦即2147483647) 的情况下，IE8 并不符合ECMAScript (第3 版) 规格。当使用大于2147483647 的索引建立数组项目时，新建立项目的索引将会是负整数。

Internet Explorer 8 does not conform to the ECMAScript (3rd edition) specification in situations where the initial value of the array’s length property is greater than 2E+31-1 (that is, 2147483647). When an array element is created with an index greater than 2147483647, the index of new element created will be a negative integer.

IE9 会正确处理使用介于2E+31-1 和2E+32-2 指数之间的数组项目。 IE9 的任何文件模式中并未复写IE8 行为。

Internet Explorer 9 correctly handles array elements that use indices between 2E+31-1 and 2E+32-2\. The Internet Explorer 8 behavior is not replicated in any of the Internet Explorer 9 document modes.

当项目推送到长度为2E+31-1 的数组时，即可观察到此现象。

This can be observed when an element is pushed to an array of length 2E+31-1.

下例在IE8 输出“true”，但是在IE9 输出“false”。

The following sample prints “true” in Internet Explorer 8, but prints “false” in Internet Explorer 9.
```javascript
function test() {
  var arr = new Array();
  arr[2147483650] = 10000;
  arr.push(10);
  document.write(arr["-2147483645"] == 10);
}
test();
```
**解决方案及正确写法**:

需要注意此项差异即可。

**相关资源：**

1.重叠元素会被复制。
MSDN原文：Overlapping Elements Are Cloned.

| 所属分类 | 版本更新 |
| ------ |-------|
| Javascript and DOM   | IE8-IE9 |

**具体描述及示例**：

在IE9 中，重叠的格式化元素是会被复制的，其用意是为了减低在DOM (Document Object Model) 中语意模糊的情况。

一般来说，网站有没有这项功能看起来都一样。不过，若是您对标记中的重叠元素执行DOM 操作，该动作在IE9 中的运作方式可能不一样。

举例来说，假如您对重叠元件进行像是firstChild或nextSibling这类的DOM呼叫，这些呼叫可能不会以相同的方式运作。

Overlapping formatting elements are cloned in Windows Internet Explorer 9 to reduce ambiguity in the Document Object Model (DOM).

Typically, a site looks the same with or without this feature. However, if you perform DOM operations on elements that are overlapped in markup, the operations might not work the same way in Internet Explorer 9.

For example, if you make DOM calls such as firstChild or nextSibling on an overlapped element, these calls might not work the same way.

**解决方案及正确写法**:

不过此问题的优先级为”Low”，所以我们可以不必太在意。

使用IE开发者工具来测试您的程式码，以找出并修正所有的重叠元素。

Test your code by using the Internet Explorer Developer Tools to find and fix any overlapping elements.

**相关资源：**

1.DOM中会保留空格。
MSDN原文：White Spaces Are Preserved in the Document Object Model.

| 所属分类 | 版本更新 |
| ------ |-------|
| Javascript and DOM   | IE8-IE9 |

**具体描述及示例**：

您新增的任何空白字符都会保存在DOM中。

Any white space that you add to a webpage persists in the Document Object Model (DOM).

請考虑下面的例子在IE9 与IE8 中的差异。

Consider how the following code example appears in Windows Internet Explorer 9 and Windows Internet Explorer 8.
```html
<div>Text </div>
```

IE9：
div
|->"\n Text "

IE8：

div

|->"Text"


考虑如下代码：
```html
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<title></title>
</head>
<body>
  <div>
    <input type="input" id="t1" value="t1" />
  </div>
</body>
</html>
```

在IE8下文档结构图如下：



在IE9下文档结构图如下：



**解决方案及正确写法**:

请留意代码中的空格。

**相关资源：**


1.  部分DOM 事件已过时。

MSDN原文：Some DOM Events Are Deprecated。

| 所属分类 | 版本更新 |
| ------ |-------|
| Javascript and DOM   | IE8-IE9 |

**具体描述及示例**：

部分DOM 事件已过时。

Microsoft identifies features as deprecated to warn against using the features in the future so that they can be phased out:


•If new standards-based support is added in Windows Internet Explorer 9, the associated feature documentation on MSDN is marked with "deprecated" and includes a link to the standards-based alternative.

•If the feature is a web platform feature, it is supported across all document modes until the feature is shown to be used by very few websites.

The following Document Object Model (DOM) events features are deprecated in IE9 standards document mode and are intended to be removed in the latest standards mode of the next major release.

**Deprecated feature**

**W3C standards replacement**

[attachEvent](http://go.microsoft.com/fwlink/?LinkID=190906)
[addEventListener](http://go.microsoft.com/fwlink/?LinkID=201619)
[detachEvent](http://go.microsoft.com/fwlink/?LinkId=201620)
[removeEventListener](http://go.microsoft.com/fwlink/?LinkID=201621)
[createEventObject](http://go.microsoft.com/fwlink/?LinkID=201624)
[createEvent](http://go.microsoft.com/fwlink/?LinkId=201622)
[fireEvent](http://go.microsoft.com/fwlink/?LinkId=201625)
[dispatchEvent](http://go.microsoft.com/fwlink/?LinkId=201623)


**解决方案及正确写法**:

建议写法：
```html
<html xmlns="http://www.w3.org/1999/xhtml">
<head id="Head1" runat="server">
<script type="text/javascript">
function LoadEverything() {
  //应停用过时方法
  //document.getElementById("btnAdd").attachEvent("onclick", ShowHello);
  //应用标准方法，注意事件不是onclick，而是click.
  document.getElementById("btnAdd").addEventListener("click", ShowHello);
}

function ShowHello() {
  alert("hello");
}
</script>
</head>
<body onload="LoadEverything()">
  <form id="form1" runat="server">
    <input type="button" id="btnAdd" value="btnAdd"  />
  </form>
</body>
</html>
```
**相关资源：**


** **

# 第四章：其他更新

## 第一节：IE7-IE8更新

1.文件上载控件仅向服务器提交文件路径，而不提供完整路径。
MSDN原文：File upload control only submits the file path, not the full path, to the server.

| 所属分类 | 版本更新 |
| ------ |-------|
| 其他   | IE7-IE8更新 |

**具体描述及示例**：

文件上传控件仅向服务器提交文件路径，而不提供完整路径.

Historically, the HTML File Upload Control (`<input type=file>`) has been the source of a significant number of information disclosure vulnerabilities. To resolve these issues, two changes were made to the behavior of the control.

To block attacks that rely on “stealing” keystrokes to surreptitiously trick the user into typing a local file path into the control, the File Path edit box is now read-only. The user must explicitly select a file for upload using the File Browse dialog.

Additionally, the “Include local directory path when uploading files” URLAction has been set to "Disable" for the Internet Zone. This change prevents leakage of potentially sensitive local file-system information to the Internet. For instance, rather than submitting the full path C:\users\ericlaw\documents\secret\image.png, Internet Explorer 8 will now submit only the filename image.png.

**解决方案及正确写法**:

注意此特性即可。

**相关资源：**

http://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx


## 第二节：IE8-IE9更新

1.预设使用者代理(UA) 字串已变更.

MSDN原文：Default User-Agent (UA) String Changed.

| 所属分类 | 版本更新 |
| ------ |-------|
| 其他   | IE8-IE9 |

**具体描述及示例**：

IE9 使用缩短的UA使用者代理字符串, 不会再传送安装在电脑上的其他软件所新增的UA 字串，例如.NET Framework 。

By default, Internet Explorer 9 sends a new short UA string to improve the overall performance, interoperability, and compatibility. Internet Explorer 9 does not send additions to the UA string that are made by other software that is installed on the computer, such as the .NET Framework and many other programs.

IE7: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0; Trident/5.0; SLCC1; .NET CLR 2.0.50727; .NET CLR 3.0.30729; .NET CLR 3.5.30729; .NET4.0C; .NET4.0E)

IE9: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)

**解决方案及正确写法**:

需要注意此项差异。
