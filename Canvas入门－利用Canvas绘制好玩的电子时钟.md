# Canvas入门－利用Canvas绘制好玩的电子时钟
## 在这之前
你需要了解一下方法的使用：

- `beginPath()`
- `closePath()`
- `moveTo()`
- `lineTo()`
- `fill()`
- `stroke()`
- `fillRect()`
- `clearRect()`

这些我在前面的文章介绍过，可以看：

[canvas入门－利用 canvas 制作一个七巧板](https://segmentfault.com/a/1190000005953758)

## 画个圆
### `arc()`方法

```js
arc(x, y, radius, startAngle, endAngle, anticlockwise)
```
= > 画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为顺时针）来生成。  
该方法有五个参数：x,y为绘制圆弧所在圆上的圆心坐标。radius为半径。startAngle以及endAngle参数用弧度定义了开始以及结束的弧度。这些都是以x轴为基准。参数anticlockwise 为一个布尔值。为true时，是逆时针方向，否则顺时针方向。
> 注意：arc()函数中的角度单位是弧度，不是度数。角度与弧度的js表达式:radians=(Math.PI/180)*degrees。

```js
//画一个带边框的实心圆
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");
canvas.width = 600;
canvas.height = 600;
ctx.beginPath();
var x = 200, // x 坐标值
	y = 200, // y 坐标值
	radius = 50, //半径
	startAngle = 0 ; //开始点
	endAngle = Math.PI * 2; //结束点
	anticlockwise = true; //逆时针 
ctx.arc(x, y, radius, startAngle, endAngle, anticlockwise);
ctx.lineWidth = 10;
ctx.fillStyle = "#1208ff";
ctx.strokeStyle = "#333";
ctx.stroke();
ctx.fill();
```
实现的效果图如下：
![cricle.png](http://upload-images.jianshu.io/upload_images/112419-60b056fcd4fd432d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



## 准备工作
会画个圆之后呢，就要开始绘制我们的电子时钟。开始之前，我们需要理清思路。首先，我们要创建个二维数组放置我们从0-9的点阵图形，当元素的值为1的时候，就要将其绘制出来。以下是二维数组的片段：

```js
[
    [0,0,1,1,1,0,0],
    [0,1,1,0,1,1,0],
    [1,1,0,0,0,1,1],
    [1,1,0,0,0,1,1],
    [1,1,0,0,0,1,1],
    [1,1,0,0,0,1,1],
    [1,1,0,0,0,1,1],
    [1,1,0,0,0,1,1],
    [0,1,1,0,1,1,0],
    [0,0,1,1,1,0,0]
]//0
```

我们要做的就是将 0 － 9 个数字用二维数组表示出来。

## 绘制电子时钟的数字
首先，我们要遍历我们的二维数组，如果元素的值为 1 ，则我们就将他绘制成圆形，那如何确定每个元素的圆心呢，看下面这张图：

![确定圆心的坐标](http://upload-images.jianshu.io/upload_images/112419-4576b11945079f1a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接着我们写这样一个绘制数字的函数：

```js
function drawNum(x, y, num, ctx){ 
	for (var i = 0; i < digit[num].length; i++){
		for(var j = 0; j < digit[num][i].length; j++){
			if( digit[num][i][j] == 1){
				ctx.beginPath();
				ctx.fillStyle = "rgb(0, 102, 153)";
				ctx.arc(x+(RADIUS+1)*2*j+(RADIUS+1), y+(RADIUS+1)*2*i+(RADIUS+1), RADIUS, 0, Math.PI*2);
				ctx.fill();
				ctx.closePath();
			}
		}
	}
}
```

然后，调用该函数来绘制我们的数字：

```js
var RADIUS = 4; // 圆的半径
drawNum(0, 0, 1, ctx);
```

绘制的效果如下：
![1.png](http://upload-images.jianshu.io/upload_images/112419-3519649215b96c9f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240))

## 绘制简单的电子时钟
我们需要定义一个函数`draw()`来绘制我们的电子时钟。

```js
function draw(ctx){
	var curDate = new Date();
	var hour = curDate.getHours();
	var minute = curDate.getMinutes();
	var seconds = curDate.getSeconds();
	drawNum(Margin_X, Margin_Y, parseInt(hour/10), ctx);
	drawNum(Margin_X + (RADIUS+1)*15, Margin_Y, parseInt(hour%10), ctx);
	drawNum(Margin_X + (RADIUS+1)*30, Margin_Y, 10, ctx);
	drawNum(Margin_X + (RADIUS+1)*45, Margin_Y, parseInt(minute/10), ctx);
	drawNum(Margin_X + (RADIUS+1)*60, Margin_Y, parseInt(minute%10), ctx);
	drawNum(Margin_X + (RADIUS+1)*75, Margin_Y, 10, ctx);
	drawNum(Margin_X + (RADIUS+1)*90, Margin_Y, parseInt(seconds/10), ctx);
	drawNum(Margin_X + (RADIUS+1)*105, Margin_Y, parseInt(seconds%10), ctx);
}
```
为了让每个数字之间有些间隔，不重叠在一起，定义了两个变量 `Margin_X`和`Margin_Y`来控制它距画布左边和顶部的距离。初始值都是30；

```js
var Margin_X = 30;  //  离 canvas 原点的坐标值 x
var Margin_Y = 30;  //  离 canvas 原点的坐标值 y
```


## 让电子时钟动起来
### `setInterval()`方法

```js
setInterval(function,time)
```
= > 该方法会循环执行一个函数，时间间隔为 time（ms）

我们利用 `setInterval`方法让我们的电子时钟动起来。

```js
setInterval(function(){
		draw(ctx);
}, 500);
```
相应的，我们需要在重复绘制前，清楚我们的画布，不然会导致数字重叠在一起。这里用到了`clearRect()`清楚我们的画布。

```js
ctx.clearRect(0,0,1024,786);
```

附上完整的代码：

```js
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<title>canvas</title>
</head>
<body>
	<canvas id="canvas" width = "1024" height= "786" style="display: block; margin: 50px auto;" >
		当前浏览器不支持canvas	
	</canvas>
<script src="digit.js"></script>
<script type="text/javascript">
//电子时钟

var RADIUS = 4;  // 圆的半径
var Margin_X = 30;  //  离 canvas 原点的坐标值 x
var Margin_Y = 30;  //  离 canvas 原点的坐标值 y

window.onload = function(){
	var canvas = document.getElementById("canvas");
	var ctx = canvas.getContext("2d");
	setInterval(function(){
		draw(ctx);
	}, 500);
}

function draw(ctx){
	ctx.clearRect(0,0,1024,786);
	var curDate = new Date();
	var hour = curDate.getHours();
	var minute = curDate.getMinutes();
	var seconds = curDate.getSeconds();
	drawNum(Margin_X, Margin_Y, parseInt(hour/10), ctx);
	drawNum(Margin_X + (RADIUS+1)*15, Margin_Y, parseInt(hour%10), ctx);
	drawNum(Margin_X + (RADIUS+1)*30, Margin_Y, 10, ctx);
	drawNum(Margin_X + (RADIUS+1)*45, Margin_Y, parseInt(minute/10), ctx);
	drawNum(Margin_X + (RADIUS+1)*60, Margin_Y, parseInt(minute%10), ctx);
	drawNum(Margin_X + (RADIUS+1)*75, Margin_Y, 10, ctx);
	drawNum(Margin_X + (RADIUS+1)*90, Margin_Y, parseInt(seconds/10), ctx);
	drawNum(Margin_X + (RADIUS+1)*105, Margin_Y, parseInt(seconds%10), ctx);
}

function drawNum(x, y, num, ctx){
	for (var i = 0; i < digit[num].length; i++){
		for(var j = 0; j < digit[num][i].length; j++){
			if( digit[num][i][j] == 1){
				ctx.beginPath();
				ctx.fillStyle = "rgb(0, 102, 153)";
				ctx.arc(x+(RADIUS+1)*2*j+(RADIUS+1), y+(RADIUS+1)*2*i+(RADIUS+1), RADIUS, 0, Math.PI*2);
				ctx.fill();
				ctx.closePath();
			}
		}
	}
}
</script>
</body>
</html>
```

digit.js

```js
digit =
    [
        [
            [0,0,1,1,1,0,0],
            [0,1,1,0,1,1,0],
            [1,1,0,0,0,1,1],
            [1,1,0,0,0,1,1],
            [1,1,0,0,0,1,1],
            [1,1,0,0,0,1,1],
            [1,1,0,0,0,1,1],
            [1,1,0,0,0,1,1],
            [0,1,1,0,1,1,0],
            [0,0,1,1,1,0,0]
        ],//0
        [
            [0,0,0,1,1,0,0],
            [0,1,1,1,1,0,0],
            [0,0,0,1,1,0,0],
            [0,0,0,1,1,0,0],
            [0,0,0,1,1,0,0],
            [0,0,0,1,1,0,0],
            [0,0,0,1,1,0,0],
            [0,0,0,1,1,0,0],
            [0,0,0,1,1,0,0],
            [1,1,1,1,1,1,1]
        ],//1
        [
            [0,1,1,1,1,1,0],
            [1,1,0,0,0,1,1],
            [0,0,0,0,0,1,1],
            [0,0,0,0,1,1,0],
            [0,0,0,1,1,0,0],
            [0,0,1,1,0,0,0],
            [0,1,1,0,0,0,0],
            [1,1,0,0,0,0,0],
            [1,1,0,0,0,1,1],
            [1,1,1,1,1,1,1]
        ],//2
        [
            [1,1,1,1,1,1,1],
            [0,0,0,0,0,1,1],
            [0,0,0,0,1,1,0],
            [0,0,0,1,1,0,0],
            [0,0,1,1,1,0,0],
            [0,0,0,0,1,1,0],
            [0,0,0,0,0,1,1],
            [0,0,0,0,0,1,1],
            [1,1,0,0,0,1,1],
            [0,1,1,1,1,1,0]
        ],//3
        [
            [0,0,0,0,1,1,0],
            [0,0,0,1,1,1,0],
            [0,0,1,1,1,1,0],
            [0,1,1,0,1,1,0],
            [1,1,0,0,1,1,0],
            [1,1,1,1,1,1,1],
            [0,0,0,0,1,1,0],
            [0,0,0,0,1,1,0],
            [0,0,0,0,1,1,0],
            [0,0,0,1,1,1,1]
        ],//4
        [
            [1,1,1,1,1,1,1],
            [1,1,0,0,0,0,0],
            [1,1,0,0,0,0,0],
            [1,1,1,1,1,1,0],
            [0,0,0,0,0,1,1],
            [0,0,0,0,0,1,1],
            [0,0,0,0,0,1,1],
            [0,0,0,0,0,1,1],
            [1,1,0,0,0,1,1],
            [0,1,1,1,1,1,0]
        ],//5
        [
            [0,0,0,0,1,1,0],
            [0,0,1,1,0,0,0],
            [0,1,1,0,0,0,0],
            [1,1,0,0,0,0,0],
            [1,1,0,1,1,1,0],
            [1,1,0,0,0,1,1],
            [1,1,0,0,0,1,1],
            [1,1,0,0,0,1,1],
            [1,1,0,0,0,1,1],
            [0,1,1,1,1,1,0]
        ],//6
        [
            [1,1,1,1,1,1,1],
            [1,1,0,0,0,1,1],
            [0,0,0,0,1,1,0],
            [0,0,0,0,1,1,0],
            [0,0,0,1,1,0,0],
            [0,0,0,1,1,0,0],
            [0,0,1,1,0,0,0],
            [0,0,1,1,0,0,0],
            [0,0,1,1,0,0,0],
            [0,0,1,1,0,0,0]
        ],//7
        [
            [0,1,1,1,1,1,0],
            [1,1,0,0,0,1,1],
            [1,1,0,0,0,1,1],
            [1,1,0,0,0,1,1],
            [0,1,1,1,1,1,0],
            [1,1,0,0,0,1,1],
            [1,1,0,0,0,1,1],
            [1,1,0,0,0,1,1],
            [1,1,0,0,0,1,1],
            [0,1,1,1,1,1,0]
        ],//8
        [
            [0,1,1,1,1,1,0],
            [1,1,0,0,0,1,1],
            [1,1,0,0,0,1,1],
            [1,1,0,0,0,1,1],
            [0,1,1,1,0,1,1],
            [0,0,0,0,0,1,1],
            [0,0,0,0,0,1,1],
            [0,0,0,0,1,1,0],
            [0,0,0,1,1,0,0],
            [0,1,1,0,0,0,0]
        ],//9
        [
            [0,0,0,0],
            [0,0,0,0],
            [0,1,1,0],
            [0,1,1,0],
            [0,0,0,0],
            [0,0,0,0],
            [0,1,1,0],
            [0,1,1,0],
            [0,0,0,0],
            [0,0,0,0]
        ]//:
    ];
```

实现的效果图如下：
![效果图](http://upload-images.jianshu.io/upload_images/112419-710b81fea1d7ce1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

甚至我们还可以这样子：


![呜呜呜](http://upload-images.jianshu.io/upload_images/112419-66161da8b6be4e63.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)