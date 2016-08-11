# CSS查漏补缺

## 块级格式上下文（Block formatting context）

1. 普通流(Normal Flow)
	
	在普通流中，元素按照其在 HTML 中的先后位置至上而下布局，在这个过程中，行内元素水平排列，直到当行被占满然后换行，块级元素则会被渲染为完整的一个新行， 除非另外指定，否则所有元素默认都是普通流定位，也可以说，普通流中元素的位置由该元素在 HTML 文档中的位置决定。

2. 浮动 (Floats)

	在浮动布局中，元素首先按照普通流的位置出现，然后根据浮动的方向尽可能的向左边或右边偏移，其效果与印刷排版中的文本环绕相似。

3. 绝对定位 (Absolute Positioning)

	在绝对定位布局中，元素会整体脱离普通流，因此绝对定位元素不会对其兄弟元素造成影响（如果看了上文的童鞋，会发现这点与浮动元素会影响兄弟元素是不同的），而元素具体的位置由绝对定位的坐标决定。

BFC 正是属于普通流的，因此它对兄弟元素也不会造成什么影响。

### 什么是BFC?
> 块格式化上下文（block formatting context） 是页面 CSS 视觉渲染的一部分。它是用于决定块盒子的布局及浮动相互影响的一个区域。 --[MDN 块格式上下文](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)

我的理解是，BFC是一个环境，在这个环境中的元素不会影响到其他环境中的布局，也就是说，处于不同BFC中的元素是不会互相干扰的。

### BFC的触发条件

- 根元素或其它包含它的元素
- 浮动元素，float除none以外的值
- 绝对定位元素 (元素的 position 为 absolute 或 fixed)
- display为以下其中之一的值：inline-block,table-cell,table-caption
- overflow 的值不为 visible的元素
- 弹性盒子 flex boxes (元素的 display: flex 或 inline-flex)

其中，最常见的就是overflow:hidden、float:left/right、position:absolute。也就是说，每次看到这些属性的时候，就代表了该元素以及创建了一个BFC了。

### BFC的特性
1. 内部的盒会在垂直方向一个接一个排列（可以看作BFC中有一个的常规流）；
2. 处于同一个BFC中的元素相互影响，可能会发生margin collapse；
3. 每个元素的margin box的左边，与容器块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此；
4. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然；
5. 计算BFC的高度时，考虑BFC所包含的所有元素，连浮动元素也参与计算；
6. 浮动盒区域不叠加到BFC上；


### BFC有什么用？
1. 阻止外边距折叠

	两个相连的块级元素在垂直上的外边距会发生叠加，有些把这种情况看作是bug，但我觉得可能是出于段落排版的考虑，为了令行间距一致才有的这一特性。我们先来看看例子：
	
	```html
	<p>first</p>
	<p>second</p>
	```
	
	```css
    *{margin: 0px;padding: 0px}
    p {
        color: red;
        background: #eee;
        width: 100px;
        height: 100px;
        line-height: 100px;
        text-align: center;
        margin: 10px;
        border: solid 1px red;
    }
	```
	
	![外边距折叠情况](http://upload-images.jianshu.io/upload_images/112419-718d5edcacc40ffb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

	从上面可以看出，我们给两个p元素都设置`margin`,但中间的间距却发生了折叠。然后举个BFC的例子：
	
	```css
	.ele{
        overflow: hidden;
        border: solid 1px red;
    }
	```
	
	```html
	<div class="ele">
	    <p>first</p>
	</div>
	<div class="ele">
	    <p>second</p>
	</div>
	```
	![防止外边距折叠](http://upload-images.jianshu.io/upload_images/112419-a856a6ee0a87c8ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
	
	从上面可以看出，我们为每个div元素设置`overflow`的值为`hidden`,产生一个块级格式上下文，因为外边距不会相互重叠。
2. BFC可以包含浮动的元素
	
	```html
	//html
	<div class = "box">
		<div class= "floatL">float</div>
		<div class= "floatL">float</div>
	</div>
	
	<br style="clear:both">
	<div class = "box BFC">
		<div class= "floatL">float</div>
		<div class= "floatL">float</div>
	</div>
	```
	
	```css
    *{margin: 0px;padding: 0px}
    .floatL{
    	float: left;
    	width: 100px;
    	height: 100px;
    	background-color: red;
    	text-align: center;
    	line-height: 100px;
    }
    .box{
    	border: 1px solid red;
    	width: 300px;
    	margin: 100px;
    	padding: 20px;
    }
    .BFC{
    	overflow: hidden;
    	*zoom: 1;
    }
	```
	![bfc计算高度](http://upload-images.jianshu.io/upload_images/112419-11fba107598d06e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
	
	从运行结果可以看出，如果块级元素里面包含着浮动元素会发生高度塌陷，但是将它变成一个BFC后，BFC在计算高度时会自动将浮动元素计算在内。
	
3. BFC可以阻止元素被浮动元素覆盖
	
	```html
	<div class="box1">box1</div>
	<div class="box2">box2</div>
	```
	
	```css
    *{margin: 0px; padding: 0px}

    .box1{
    	width: 100px;
    	height: 100px;
    	line-height: 100px;
    	text-align: center;
    	background-color: rgba(0, 0, 255, 0.5);
    	border: 1px solid #000;
    	float: left;
    }
    .box2{
    	width: 200px;
    	height: 200px;
    	line-height: 100px;
    	text-align: center;
    	background-color: rgba(255, 0, 0, 0.5);
    	border: 1px solid #000;
    	/* overflow: hidden; */
    	/* *zoom: 1; */
    }
	```
	![浮动元素重叠](http://upload-images.jianshu.io/upload_images/112419-eb22d5bc72038736.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

	从上面看出，当元素浮动后，会与后面的块级元素产生相互覆盖。那怎么解决这个问题，只要为后面的元素创建一个BFC。添加`overflow`属性到`box2`上。
	
	```css
	overflow: hidden;
	*zoom: 1;
	```
	
	![bfc](http://upload-images.jianshu.io/upload_images/112419-fbf4d4824e4db026.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
	
	这样子阻止了浮动元素重叠的问题。
	
### BFC与`hasLayout`
除了使用 `overflow: hidden` 触发 BFC 外，还使用了一个 `*zomm: 1` 的属性，这是 IEhack ，因为 IE6-7 并不支持 W3C 的 BFC ，而是使用私有属性 `hasLayout` 。从表现上来说，`hasLayout `跟 BFC 很相似，只是 `hasLayout` 自身存在很多问题，导致了 IE6-7 中一系列的 bug 。触发 hasLayout 的条件与触发 BFC 有些相似，推荐为元素设置 IE 特有的 CSS 属性 `zoom: 1` 触发 `hasLayout` ，`zoom` 用于设置或检索元素的缩放比例，值为“1”即使用元素的实际尺寸，使用 `zoom: 1` 既可以触发 hasLayout 又不会对元素造成其他影响，相对来说会更为方便。

拓展阅读：

[Block formatting context(块级格式化上下文)](http://www.cnblogs.com/MockingBirdHome/p/3365346.html)

[学习块格式化上下文](http://mp.weixin.qq.com/s?src=3&timestamp=1470323942&ver=1&signature=Y72Cy9gjrTcOj6VGI7-BPdSdo5Zi6iSCYmISpw274JuHSaCizBF6Qik-sGs-dxvXrHGtLoaKnuIjjCGlp30YMWyYSq*DvSWyYuqIcaedAjGXgJXa8WImqJlo9BexKzlF2t9Cb*KCg5yjrJo*51Nc8g==)

[BFC与hasLayout](http://www.cnblogs.com/pigtail/archive/2013/01/23/2871627.html)



## 清除浮动
经典的清除浮动：

```css
//利用伪元素清除浮动
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

拓展阅读：
[那些年一起清除过的浮动](http://www.cnblogs.com/lhb25/p/story-of-clear-float.html)


## 认识圣杯布局和双飞翼布局
各种各样的布局，无非就是用了浮动 float，负边距，相对定位，通过这三者的巧妙组合跟拼凑来实现的。用好这些，布局就会很简单。

还没学会布局时，就听到有圣杯布局和双飞翼布局，这布局都有这么风骚的名字，就觉得很酷，事实也如此，了解了圣杯布局和双飞翼布局，才发现挺深奥的。

传统的布局中，当我们需要改变两栏的互换，就会很麻烦。因为还要涉及到 HTML 代码的修改，不能完全从 CSS 上更改，这叫 HTML 和 CSS 的耦合。而圣杯布局跟双飞翼布局就是能够不考虑主体的位置，能够只通过 CSS 代码就改变相应的布局，这也是优点之一。

### 圣杯布局

试试这样的HTML结构：

```html
<div class="header">header</div>
<div class="container">
    <div class="main">main</div>
    <div class="sub">sub</div>
    <div class="extra">extra</div>
</div>
<div class="footer">footer</div>
```

给它加上CSS样式：

```css
body{ margin: 0; padding: 0; font-size: 1.5em; font-weight: bold; min-width: 500px;}
.header,.footer{ text-align: center;}
.header{ height: 50px; background-color: #76ffff;}
.footer{ height: 50px; background-color: #ff7676;}
.main{ background-color: #666;}
.sub{ background-color: #44fa44;}
.extra{ background-color: #3dbdff;}
/*start*/
.main{
    width: 100%;
    float: left;
}
.sub{
    width: 100px;
    float: left;
    margin-left: -100%;
}
.extra{
    width: 200px;
    float: left;
    margin-left: -200px;
}
.container{
    overflow: hidden;//BFC，撑高高度
}
```
结果如下：
![011542497982872.png](http://upload-images.jianshu.io/upload_images/112419-2072bb4dcd5f9f25.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

会发现，main的位置不正确，所以再给它加上 `padding: 0 200px 0 100px`;

![011544456578941.png](http://upload-images.jianshu.io/upload_images/112419-7bc25265fadca623.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

虽然 main 的位置正确了，可是 sub 和 extra 位置优点不对，所以我们再用上相对定位，为 sub 和 extra 加上如下代码：

```css
.sub{
    position: relative;;
    left: -100px;
}
.extra{
    position: relative;
    right: -200px;
}
```
效果就出来了，
![011547019548969.png](http://upload-images.jianshu.io/upload_images/112419-bcab4443f3dbb1cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

噢耶，这就是圣杯布局。如果在圣杯布局的基础上，给它一个多余的标签，把 mian 包起来，这就是双飞翼布局。

### 双飞翼布局

HTML结构：

```html
<div class="header">header</div>
<div class="container">
    <div class="main">
        <div class="main-wrap">main</div>
    </div>
    <div class="sub">sub</div>
    <div class="extra">extra</div>
</div>
<div class="footer">footer</div>
```
CSS结构：

```css
body{ margin: 0; padding: 0; font-size: 1.5em; font-weight: bold; min-width: 500px;}
.header,.footer{ text-align: center;}
.header{ height: 50px; background-color: #76ffff;}
.footer{ height: 50px; background-color: #ff7676;}
.main{ background-color: #666;}
.sub{ background-color: #44fa44;}
.extra{ background-color: #3dbdff;}
/*start*/
.main{
    width: 100%;
    height: 100px;
    float: left;
}
.sub{
    width: 100px;
    height: 100px;
    float: left;
    margin-left: -100%;
}
.extra{
    width: 200px;
    height: 100px;
    float: left;
    margin-left: -200px;
}
.main-wrap{
    margin: 0 200px 0 100px;
}
.container{
    height: 100px;
    overflow: hidden;
    *zoom: 1;
}
```
可以看到，只要为包住 main-wrap 设置 margin，连相对定位都没用到，效果就出来了。
![011558067042018.png](http://upload-images.jianshu.io/upload_images/112419-a4a678a15ff3393e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 如果把三栏布局比作一只大鸟，可以把main看成是鸟的身体，sub和extra则是鸟的翅膀。这个布局的实现思路是，先把最重要的身体部分放好，然后再将翅膀移动到适当的地方。因此请容许我给这个布局实现取名为双飞翼布局（Flying Swing Layout）.
就如上图中的鸟有各种姿势一样，利用双飞翼布局，我们也可以实现各种布局。这里有个尝试页面，利用双飞翼，实现了一套栅格化布局系统。

优点：

- 实现了内容与布局的分离，这是渐进式增强布局的思想，从内容出发，不考虑布局。
- main部分是自适应宽度的，很容易在定宽布局和流体布局中切换。
- 任何一栏都可以是最高栏，不会出问题。
- 需要的hack非常少（就一个针对ie6的清除浮动hack:_zoom: 1;）
在浏览器上的兼容性非常好，IE5.5以上都支持。

缺点：

- main需要一个额外的包裹层。

## normalize.css和reset.css
normalize 的理念则是尽量保留浏览器的默认样式，不进行太多的重置。

reset 的目的，是将所有的浏览器的自带样式重置掉，这样更易于保持各浏览器渲染的一致性。

```css
/* reset */
html,body,h1,h2,h3,h4,h5,h6,div,dl,dt,dd,ul,ol,li,p,blockquote,pre,hr,figure,table,caption,th,td,form,fieldset,legend,input,button,textarea,menu{margin:0;padding:0;}
header,footer,section,article,aside,nav,hgroup,address,figure,figcaption,menu,details{display:block;}
table{border-collapse:collapse;border-spacing:0;}
caption,th{text-align:left;font-weight:normal;}
html,body,fieldset,img,iframe,abbr{border:0;}
i,cite,em,var,address,dfn{font-style:normal;}
[hidefocus],summary{outline:0;}
li{list-style:none;}
h1,h2,h3,h4,h5,h6,small{font-size:100%;}
sup,sub{font-size:83%;}
pre,code,kbd,samp{font-family:inherit;}
q:before,q:after{content:none;}
textarea{overflow:auto;resize:none;}
label,summary{cursor:default;}
a,button{cursor:pointer;}
h1,h2,h3,h4,h5,h6,em,strong,b{font-weight:bold;}
del,ins,u,s,a,a:hover{text-decoration:none;}
body,textarea,input,button,select,keygen,legend{font:12px/1.14 arial,\5b8b\4f53;color:#333;outline:0;}
body{background:#fff;}
a,a:hover{color:#333;}
```

以上reset来自[NEC](http://nec.netease.com/framework/css-reset.html)的css reset。

拓展阅读：
[Normalize.css 和 Reset CSS 有什么本质区别？](https://segmentfault.com/q/1010000000117189)

## IE条件注释
IE条件注释是一种特殊的HTML注释，这种注释只有IE5.0及以上版本才能理解。比如普通的HTML注释是：

```html
<!--This is a comment-->
　　而只有IE可读的IE条件注释是：
<!--[if IE]> <![endif]-->
　　“非IE条件注释”：
<!--[if !IE]>--> non-IE HTML Code <!--<![endif]-->
　　“非特定版本IE条件注释”（很少用到）：
<!--[if ! lt IE 7]>
<![IGNORE[--><![IGNORE[]]>
Code for browsers that match the if condition
<!--<![endif]-->
```
简而言之，除了“Windows上的IE”之外的所有浏览器都会认为条件注释只是一段普通的HTML注释。你不能在CSS代码中使用条件注释。IE条件注释是很有用的对IE隐藏或者展现特定代码的方法，比起在CSS中用诡异的_/制造bug，利用IE条件注释来写CSS “hacks”是更合理的方法。通俗点，条件注释就是一些if判断，但这些判断不是在脚本里执行的，而是直接在html代码里执行的。

1. 条件注释的基本结构和HTML的注释(<!– –>)是一样的。因此IE以外的浏览器将会把它们看作是普通的注释而完全忽略它们。
2. IE将会根据if条件来判断是否如解析普通的页面内容一样解析条件注释里的内容。
3. 条件注释使用的是HTML的注释结构，因此他们只能使用在HTML文件里，而不能在CSS文件中使用。

从语法上看这是相当合法的普通HTML注释。任何浏览器都会认为<!–和–>之间的部分是注释从而忽略它。但是IE也会看到其中[if IE]>，从而开始解释接下来的代码直到遇到<![endif]。所以，下面这些代码不会显示在任何其他浏览器中面。

通过“比较操作符”可以更灵活地对IE版本进行控制，用法是在IE前面加上“比较操作符”。合法的操作符如下：

- lte：就是Less than or equal to的简写，也就是小于或等于的意思。
- lt ：就是Less than的简写，也就是小于的意思。
- gte：就是Greater than or equal to的简写，也就是大于或等于的意思。
- gt ：就是Greater than的简写，也就是大于的意思。
- ! ：就是不等于的意思，跟javascript里的不等于判断符相同

```html
<!–[if gt IE 5.5]> / 如果IE版本大于5.5 /
<!–[if lte IE 6]> / 如果IE版本小于等于6 /
<!–[if !IE]> / 如果浏览器不是IE /
```

常用的IE条件注释

```html
<!--[if !IE]>除IE外都可识别<![endif]-->
<!--[if IE]> 所有的IE可识别 <![endif]-->
<!--[if IE 5.0]> 只有IE5.0可以识别 <![endif]-->
<!--[if IE 5]> 仅IE5.0与IE5.5可以识别 <![endif]-->
<!--[if gt IE 5.0]> IE5.0以及IE5.0以上版本都可以识别 <![endif]-->
<!--[if IE 6]> 仅IE6可识别 <![endif]-->
<!--[if lt IE 6]> IE6以及IE6以下版本可识别 <![endif]-->
<!--[if gte IE 6]> IE6以及IE6以上版本可识别 <![endif]-->
<!--[if IE 7]> 仅IE7可识别 <![endif]-->
<!--[if lt IE 7]> IE7以及IE7以下版本可识别 <![endif]-->
<!--[if gte IE 7]> IE7以及IE7以上版本可识别 <![endif]-->
```