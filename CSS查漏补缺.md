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

```html

```




## normalize和reset.css

## IE条件注释