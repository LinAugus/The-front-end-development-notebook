# HTML 和 CSS 简易总结

## HTML
### 简介
> HTML 是一种标记语言（markup language）。它告诉浏览器如何显示内容。HTML把内容（文字，图片，语言，影片等等）和「presentation」（这个内容是如何显示，比如文字用什么颜色显示等等）分开

HTML使用预先定义的元素集合来识别内容形态。 元素包含一个以上的标记来包含或者表达内容。标记利用尖括号表示，而结束标记（用来指示内容尾端）则在前面加上斜线。

### 最简单的一个html页面

```html
<!DOCTYPE html>  // 声明文档类型
<html lang="en">  // 根标签
<head>  // 文档说明定义
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>  // 文档主体内容定义
    <h1>HTML5</h1>
</body>
</html>
```
### 语法
HTML由一对尖括号`<html></html>`括起来的标签定义内容结构。

### 常见标签及属性
#### 头部
- `meta`属性
- `<title>`标签

#### 标题和段落

```html
<h1>一级标题</h1>
<h2>二级标题</h2>
<h3>三级标题</h3>
<h4>四级标题</h4>
<h5>五级标题</h5>
<h6>六级标题</h6>
<p>这是段落</p>
```
####  超链接和图片
- a
	
	```html
<a target="_blank" href="http://www.baidu.com/">百度</a>
<a href="mailto:xxx@qq.com">联系我们</a>
<a href="#menu1">联系我们</a>
	```
	target: 打开页面方式
	
	1. _self： 默认，在当前选项卡打开，替换当前页面
	2. _blank：新的选项卡打开
	3. _parent：在父框架中打开
	4. _top: 在最顶层打开

- img

	```html
	<img width="40%" src="xxx.jpg" alt="图片替代文案"/>
	```
	属性
	
	1. src：图片地址
	2. alt： 图片因为网络等原因未成功加载时候的显示文案内容
	3. width/height：可以是像素值或者相对于父容器的百分比，两个属性仅设置一个，另外一个值会按相应等比缩放

####  有序列表和无序列表
- 有序列表

	```html
	<ol>
	    <li>Coffee</li>
	    <li>Tea</li>
	    <li>Milk</li>
	</ol>
	```
	属性

	1. start：规定起始值，默认1
	2. type：1、A、a、I、i，默认是数字
	3. reversed：降序（h5新加）
	
- 无序列表


	```html
	<ul>
	  <li>Coffee</li>
	  <li>Tea</li>
	  <li>Milk</li>
	</ul>
	```
	属性：type（不推荐使用，使用css）

	1. disc：实心原点
	2. square：方块
	3. circle：空心圆圈


#### 表格

```html
<table cellpadding="4" cellspacing="10" border="1" width="100%">
    <thead>
        <caption>Table</caption>
        <colgroup>
        <col span="2" align="left">
        <col align="right">
      </colgroup>
        <th>A</th>
        <th>B</th>
        <th>C</th>
    </thead>
    <tbody>
        <tr>
            <td align="right">00</td>
            <td valign="bottom">01</td>
            <td>02</td>
        </tr>
        <tr>
            <td>10</td>
            <td>11</td>
            <td>12</td>
        </tr>
    </tbody>
    <tfoot>
        <td>20</td>
        <td>21</td>
        <td>22</td>
    </tfoot>
</table>
```

table属性

1. border：边框
2. cellpadding：td的内边距
3. cellspacing：td的外边距
4. width：table宽度

td属性

1. align：水平对其方式
2. valign：垂直对齐方式
3. width：宽度，默认自动分布

#### iframe
iframe 也是html的一个标签，不过不是往页面上添加一个元素那么简单，iframe 会创建包含另外一个文档的内联框架（即行内框架）。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <p>下面放置一个iframe</p>

    <iframe src="./inner.html" width="500" height="300" frameborder="1"></iframe>
</body>
</html>
```

属性

1. frameborder：可用值为1和0，规定是否显示框架周围的边框
2. width：frame的宽度
3. height：frame的高度
4. src：引入的资源（页面、图片等）url

#### 表单
表单是一个包含表单元素的区域 表单元素是允许用户在表单中（比如：文本域、下拉列表、单选框、复选框等等）输入信息的元素。

```html
<form action="xxx.php" method="post">
    <h3>输入框</h3>
    <label for="input_username">用户名：</label> <input id="input_username" name="username" type="text" placeholder="输入用户名" autofocus/>
    <br/>
    <label for="input_pass">密码：</label> <input id="input_pass" name="password" type="password" placeholder="输入密码"/>
    <br/><br/>

    <h3>单选框</h3>
    <label for="ck1">ra1：</label><input type="radio" id="ra1" name="ra" value="1" />
    <br/>
    <label for="ra2">ra2：</label><input type="radio" id="ra2" name="ra" value="2" />
    <br/>
    <label for="ra3">ra3：</label><input type="radio" id="ra3" name="ra" value="3" />
    <br/>
    <label for="ra4">ra4：</label><input type="radio" id="ra4" name="ra" value="4" />
    <br/><br/>


    <h3>复选框</h3>
    <label for="ck1">CK1：</label><input type="checkbox" id="ck1" name="ck" value="1" />
    <br/>
    <label for="ck2">CK2：</label><input type="checkbox" id="ck2" name="ck" value="2" checked />
    <br/>
    <label for="ck3">CK3：</label><input type="checkbox" id="ck3" name="ck" value="3" />
    <br/>
    <label for="ck4">CK4：</label><input type="checkbox" id="ck4" name="ck" value="4" />
    <br/><br/>

    <h3>File</h3>
    <input type="file" accept="image/gif, image/jpeg"/>
    <br/><br/>

    <h3>image 按钮</h3>
    <input type="image" src="xxx.jpg" alt="Submit Form"/>
    <br/><br/>

    <h3>隐藏域</h3>
    <input type="hidden" value="xxx" />
    <br/><br/>


    <h3>下拉菜单</h3>
    <select id="input_select">
        <option value="1">1</option>
        <option value="2" selected="selected">2</option>
        <option value="3">3</option>
        <option value="4">4</option>
    </select>
    <br/><br/>

    <h3>多行文本</h3>
    <textarea cols="60" rows="5">123</textarea>
    <br/><br/>

    <input type="button" value="Button" /> &nbsp;&nbsp;
    <input type="submit" value="Submit" />  &nbsp;&nbsp;
    <input type="reset" value="Reset" />
```

- form 属性

	1. action： 表单提交的地址
	2. method：提交保单的方法
	3. target：在何处打开action
	4. enctype:
		- application/x-www-form-urlencoded：在发送前编码所有字符（默认）
		- text/plain：空格转换为 "+" 加号，但不对特殊字符编码
		- multipart/form-data：使用包含文件上传控件的表单时，必须使用该值

- checkbox
	
	靠name属性分组，提交到后端的时候被选中的value是以,分割的一个字符串，通过name属性获得
- file

	accept：mime 类型，多个以,分割
- select

	后端以select标签的name获取选中的option的value

### 块级元素和行内元素
1. HTML元素可以分为两种：

	- 块级元素(block level element）
	- 行内元素(inline element)

2. 两者的区别

	- 块级元素会独占一行（即无法与其他元素显示在同一行内，除非你显式修改元素的 display 属性），而内联元素则都会在一行内显示。
	- 块级元素可以设置 width、height 属性，而内联元素设置无效。
	- 块级元素的 width 默认为 100%，而内联元素则是根据其自身的内容或子元素来决定其宽度。

3. 常见的块级元素
`<div>``<form>``<h1-h6>``<hr>``<ul>``<ol>``<dl>`
`<p>``<table>``<pre>`

4. 常见的行内元素
`<span>``<a>``<img>``<sub>``<sup>``<br>``<b>``<em>``<label>`


## CSS
### CSS概述
> 层叠样式表 (Cascading Style Sheets)，常缩写为 CSS， 是一门 样式表 (stylesheet) 语言，用来描述 HTML、XML（包括各种 XML 语言如 SVG、XHTML）文档的呈现。CSS 描述了在屏幕、电子纸、音频或其它媒体上如何渲染元素。

> CSS 分为不同等级，CSS1 现已废弃， CSS2.1 是推荐标准， CSS3 分成多个小模块，正在标准化中。

### CSS语法
![css语法结构](/Users/lin/Downloads/3540702242-55113218bc8a5_articlex.gif)

### 在页面引用CSS样式

1. 定义在外部文件（外链样式）：可实现复用样式，推荐。

	```html
	// 在<head>中定义
	<link rel="stylesheet" type="text/css" href="xxx.css">
	```
2. 在页面的头部定义（内联样式）：通过这种形式定义的样式只在本页面内生效。

	```css
	// 在<style>中定义
	<style>
		body{
			background-color: #eee;
			font-size: 16px;
		}
	</style>
	```
3. 定义在特定的元素身上（行内样式）：这种形式多用于测试，可维护性较差。

	```css
	<p style = "font-size: 20px; ">这是段落</p>
	```

### 选择器
#### 选择器类型

1. 基础选择器
2. 组合选择器
3. 属性选择器
4. 伪类选择器
5. 伪元素选择器

#### 基础选择器
| 选择器      | 含义         |
| ---------- | -------------|
| *        | 通用元素选择器，匹配页面任何元素 |
| #id        | id选择器，匹配特定id的元素 |
| .class     | 类选择器，匹配class包含(不是等于)特定类的元素      |
| element    | 标签选择器      |

```css
* {
    font-family: '微软雅黑';
}

#id-selector{
    color: #333;
}

.class-selector{
    background: #f1f1f1;
}

p {
    height: 50px;
    line-height: 50px;
}
```

#### 组合选择器
| 选择器      | 含义         |
| ---------- | -------------|
| E,F        | 多元素选择器，用`,`分隔，同时匹配元素E或元素F |
| E F        | 后代选择器，用空格分隔，匹配E元素所有的后代（不只是子元素、子元素向下递归）元素F |
| E>F     |子元素选择器，用`>`分隔，匹配E元素的**所有直接子元素**      |
| E+F    | 直接相邻选择器，匹配E元素之后的相邻的同级元素F      |
| E~F    | 普通相邻选择器，匹配E元素之后的同级元素F（无论直接相邻与否）      |


#### 属性选择器
| 选择器      | 含义         |
| ------------- | -------------|
| E[attr]       | 匹配所有具有属性attr的元素，div[id]就能取到所有有id属性的div |
| E[attr = value]       | 匹配属性attr值为value的元素，div[id=test],匹配id=test的div |
| E[attr ~= value]     |匹配所有属性attr具有多个空格分隔、其中一个值等于value的元素      |
| E[attr ^= value]    | 匹配属性attr的值以value开头的元素      |
| E[attr $= value]   | 匹配属性attr的值以value结尾的元素      |
| E[attr *= value]   | 匹配属性attr的值包含value的元素      |


#### 伪类选择器（Pseudo-classes selectors）
> selector:pseudo-class {
  property: value;
}

| 选择器      | 含义         |
| ------------- | -------------|
| E:first-child      | 匹配元素E的第一个子元素 |
| E:link       | 匹配所有未被点击的链接 |
| E:visited     |匹配所有已被点击的链接      |
| E:active   | 匹配鼠标已经其上按下、还没有释放的E元素      |
| E:hover   | 匹配鼠标悬停其上的E元素      |
| E:lang(c)   | 匹配lang属性等于c的E元素      |
| E:enabled   | 匹配表单中可用的元素      |
| E:disabled   | 匹配表单中禁用的元素      |
| E:checked   | 匹配表单中被选中的radio或checkbox元素      |
| E:selection   | 匹配用户当前选中的元素      |
| E:root   | 匹配文档的根元素，对于HTML文档，就是HTML元素      |
| E:nth-child(n)   |匹配其父元素的第n个子元素，第一个编号为1      |
| E:nth-last-child(n)   | 匹配其父元素的倒数第n个子元素，第一个编号为1      |
| E:nth-of-type(n)   | 与:nth-child()作用类似，但是仅匹配使用同种标签的元素      |
| E:nth-last-of-type(n)   | 与:nth-last-child() 作用类似，但是仅匹配使用同种标签的元素     |
| E:last-child   | 匹配父元素的最后一个子元素，等同于:nth-last-child(1)      |
| E:first-of-type   | 匹配父元素下使用同种标签的第一个子元素，等同于:nth-of-type(1)      |
| E:last-of-type   | 匹配父元素下使用同种标签的最后一个子元素，等同于:nth-last-of-type(1)      |
| E:only-child   | 匹配父元素下仅有的一个子元素，等同于:first-child:last-child或 :nth-child(1):nth-last-child(1)      |
| E:only-of-type   | 匹配父元素下使用同种标签的唯一一个子元素，等同于:first-of-type:last-of-type或 :nth-of-type(1):nth-last-of-type(1)      |
| E:empty   | 匹配一个不包含任何子元素的元素，文本节点也被看作子元素      |
| E:not(selector)   | 匹配不符合当前选择器的任何元素      |

#### 伪元素选择器
| 选择器      | 含义         |
| ------------- | -------------|
| E::first-line      | 匹配E元素内容的第一行 |
| E::first-letter       | 匹配E元素内容的第一个字母 |
| E::before     |在E元素之前插入生成的内容      |
| E::after   | 在E元素之后插入生成的内容    |

#### CSS优先级
从高到低分别是：

1. 在属性后面使用 !important 会覆盖页面内任何位置定义的元素样式
2. 作为style属性写在元素标签上的内联样式
3. id选择器
4. 类选择器
5. 伪类选择器
6. 属性选择器
7. 标签选择器


### 文本格式化

- 
	
### 盒模型

![盒模型](/Users/lin/Downloads/2053780764-5463284ce7716_articlex.gif)

### 定位
css定位使用的是`position`属性，该属性有如下取值：

| 值      | 含义         |
| ------------- | -------------|
| inherit      | 规定应该从父元素继承 position 属性的值 |
| static       | 默认值,没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明） |
| relative     |生成相对定位的元素，相对于元素本身正常位置进行定位,因此，left:20px 会向元素的 left 位置添加20px      |
| absolute   | 生成绝对定位的元素，相对于static定位以外的第一个祖先元素（offset parent）进行定位,元素的位置通过 left, top, right 以及 bottom 属性进行规定    |
| fixed   | 生成绝对定位的元素，相对于浏览器窗口进行定位。元素的位置通过 left, top, right 以及 bottom 属性进行规定    |
| sticky   | CSS3新属性，表现类似position:relative和position:fixed的合体，在目标区域在屏幕中可见时，它的行为就像position:relative; 而当页面滚动超出目标区域时，它的表现就像position:fixed，它会固定在目标位置    |

`static` 是默认值。任意 `position: static;` 的元素不会被特殊的定位。

在一个相对定位（position属性的值为`relative`）的元素上设置 top 、 right 、 bottom 和 left 属性会使其偏离其正常位置。其他的元素则不会调整位置来弥补它偏离后剩下的空隙。

一个固定定位（position属性的值为`fixed`）元素会相对于视窗来定位，这意味着即便页面滚动，它还是会停留在相同的位置。和 relative 一样， top 、 right 、 bottom 和 left 属性都可用。

`absolute` 是最棘手的position值。 `absolute` 与 `fixed` 的表现类似，除了它不是相对于视窗而是相对于最近的“positioned”祖先元素。如果绝对定位的元素没有“positioned”祖先元素，那么它是相对于文档的 body 元素，并且它会随着页面滚动而移动。记住个“positioned”元素是指 position 值不是 static 的元素。



### 浮动
在 CSS 中，我们通过`float`属性实现元素的浮动。设置了`float`的元素会脱离普通文档流。

浮动的框可以向左或向右移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止。

由于浮动框不在文档的普通流中，所以文档的普通流中的块框表现得就像浮动框不存在一样。


#### 清除浮动
由于设置了`float`属性的元素，已经脱离了普通流，那么它的父元素不会将其计算进自身高度里面，因此就会发生“高度塌陷”现象。我们为了避免这一情况，就需要闭合浮动，也叫清除浮动。

下面是一个经典的清除浮动的例子：

```css
//利用伪元素闭合浮动
.clearfix:after {
     content:"."; 
     display:block; 
     height:0; 
     visibility:hidden; 
     clear:both; 
}
.clearfix { 
    *zoom:1; 
}
```

