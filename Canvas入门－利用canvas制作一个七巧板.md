##  简介
`<canvas>` 是 HTML5 新增的元素之一，它允许脚本语言动态渲染位图像。最初是由 Apple 引入，用于 Mac OS X 的仪表盘，后来又在 Safiri 和 Google Chrome 中被实现。 `<canvas>` 就像个神奇的画布，你可以在上面画出你想要的绚丽的效果。使用`<canvas>`元素之前，需要一些基本的 HTML 和 Javascript 知识。`<canvas>` 元素不被一些老的浏览器支持，但是主流的高级浏览器都支持。
## 基本用法
1. `<canvas>` 元素

	```html
	<canvas id = "canvas" width ="200px" height="200px“ style="border: 1px solid #0000ff; display: block; margin: 50px auto" >
您的浏览器不支持canvas！
	</canvas>
	```
如果没有设置宽度和高度的时候，canvas 会默认设置宽为300px，高为150px。当浏览器不支持 canvas 时，会将中间的文字打印出来。上面代码中，为了让画布更加明显，添加了css 样式便于查看。
2. 渲染上下文
canvas 元素创造了一个固定大小的画布，并且公开了渲染上下文对象，它可以用来绘制和处理要展示的内容。Canvas API 定义在 context这个对象上，因此要用getContext方法来获取这个对象。

	```javascript
	var canas = document.getElementById("canvas");
	// 绘制2D图像的上下文环境
	var context = canvas.getContext("2d");
	```
上面代码中，getContext方法指定参数‘2d’，表示该canvas对象绘制2d图像，执行后的效果为下图一个蓝色边框的举重的矩形。
![截图.png](http://upload-images.jianshu.io/upload_images/112419-2d2ec3e048cc50f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
3. 栅格
在绘制图像之前，我们要先了解什么是栅格，通常来说网格中的一个单元相当于canvas元素中的一像素。栅格的起点为左上角（坐标为（0,0））。所有元素的位置都相对于原点定位。所以图中蓝色方形左上角的坐标为距离左边（Y轴）x像素，距离上边（X轴）y像素（坐标为（x,y））。
![Canvas_default_grid.png](http://upload-images.jianshu.io/upload_images/112419-50c8c627a665f68c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
4. 绘制图像
好，现在可以开始绘制图像了，首先要了解下绘制 moveTo()这个方法。
- 移动笔触
```
moveTo(x, y)将笔触移动到指定的坐标x以及y上。
```
这个方法实际上并不能画出任何东西，它就是起到移动笔触的作用，并且作为起始点。
- 线
绘制直线，需要用到的方法lineTo()。
```
lineTo(x, y)绘制一条从当前位置到指定x以及y位置的直线。
```
该方法有两个参数：x以及y ，代表坐标系中直线结束的点。开始点和之前的绘制路径有关，之前路径的结束点就是接下来的开始点，等等。。。开始点也可以通过moveTo()函数改变。

	```javascript
	//绘制线条1
	context.moveTo(0,0);
	context.lineTo(100,100);
	context.stroke();
	```
![line.png](http://upload-images.jianshu.io/upload_images/112419-dbd77ef55ec0d992.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 路径
图形的基本元素是路径。路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状的点的集合。一个路径，甚至一个子路径，都是闭合的。使用路径绘制图形需要一些额外的步骤。
首先，要创建路径起始点，然后使用画图命令去画出路径，之后封闭路径。一旦路径生成，就能通过描边或填充路径区域来渲染图形。
常用到的函数：

	```
	beginPath()
	新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。
	closePath()
	闭合路径之后图形绘制命令又重新指向到上下文中。
	stroke()
	通过线条来绘制图形轮廓。
	fill()
	通过填充路径的内容区域生成实心的图形。
	```
生成路径的第一步叫做beginPath()。本质上，路径是由很多子路径构成，这些子路径都是在一个列表中，所有的子路径（线、弧形、等等）构成图形。而每次这个方法调用之后，列表清空重置，然后我们就可以重新绘制新的图形。
第二步就是调用函数指定绘制路径，本文稍后我们就能看到了。
第三，就是闭合路径closePath(),不是必需的。这个方法会通过绘制一条从当前点到开始点的直线来闭合图形。如果图形是已经闭合了的，即当前点为开始点，该函数什么也不做。
了解完这些后，我们就能够开始作出我们要的七巧板了～
## 七巧板
首先，新建一个画布

	```html
	<canvas id="canvas" style="display: block; margin: 60px auto;"></canvas>
	```
接着，为这个画布添加事件

```javascript
window.onload = function(){
			var canvas = document.getElementById("canvas");
			canvas.width = 600;
			canvas.height = 600;
			//绘制2d图像的上下文环境
			var context = canvas.getContext("2d");

			//绘制左边平行四边形
			context.beginPath();
			context.lineWidth = 5;
			context.moveTo(150,450);
			context.lineTo(450,450);
			context.lineTo(300,600);
			context.lineTo(0,600);
			context.fillStyle = "#FF1493";
			context.strokeStyle = "#000000";
			context.fill();
			context.stroke();

			//绘制左边的大三角形
			context.beginPath();
			context.lineWidth = 5;
			context.moveTo(0,0);
			context.lineTo(300,300);
			context.lineTo(0,600);
			context.fillStyle = "#0000ff";
			context.strokeStyle = "#000000";
			context.fill();
			context.stroke();

			//绘制右边的大三角形
			context.beginPath();
			context.lineWidth = 5;
			context.moveTo(0,0);
			context.lineTo(300,300);
			context.lineTo(600,0);
			context.fillStyle = "#FFFF00";
			context.strokeStyle = "#000000";
			context.fill();
			context.stroke();

			//绘制中间三角形
			context.beginPath();
			context.lineWidth = 5;
			context.moveTo(300,300);
			context.lineTo(150,450);
			context.lineTo(450,450);
			context.fillStyle = "#FF7F50";
			context.strokeStyle = "#000000";
			context.fill();
			context.stroke();


			//绘制中间小矩形
			context.beginPath();
			context.lineWidth = 5;
			context.moveTo(450,150);
			context.lineTo(300,300);
			context.lineTo(450,450);
			context.lineTo(600,300);
			context.fillStyle = "#191970";
			context.strokeStyle ="#000000";
			context.fill();
			context.stroke();

			
			//绘制最右边小三角形
			context.beginPath();
			context.lineWidth = 5;
			context.moveTo(600,0);
			context.lineTo(450,150);
			context.lineTo(600,300);
			context.fillStyle = "#A52A2A";
			context.strokeStyle = "#000000";
			context.fill();
			context.stroke();

			//绘制右边大三角形
			context.beginPath();
			context.lineWidth = 5;
			context.moveTo(600,300);
			context.lineTo(300,600);
			context.lineTo(600,600);
			context.fillStyle = "#00FFFF";
			context.strokeStyle = "#000000";
			context.fill();
			context.stroke();

			//绘制最外围的边框
			context.beginPath();//开始绘制路径
			context.lineWidth = 10;//设置线条的宽度
			context.moveTo(0,0);//移动笔触
			context.lineTo(600,0);//绘制线条
			context.lineTo(600,600);
			context.lineTo(0,600);
			context.closePath();//封闭路径
			context.stroke();//通过线条来绘制图形轮廓
		};
```
优化版本：

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<title>canvas</title>
</head>
<body>
	<!-- 优化七巧板，将模型数据用数据存储起来，逐个取出并绘制。 -->
	<canvas id="canvas" style="border: 1 px solid #aaa; display: block; margin: 50px auto">
		当前浏览器不支持cnavas		
	</canvas>
	<script type="text/javascript">
	var tangram = [
		{
			f:[{x: 0,y: 0},{x: 800, y: 0},{x: 400, y: 400}],
			color: "#FEF734",
		},
		{
			f:[{x: 0,y: 0},{x: 400, y: 400},{x: 0, y: 800}],
			color: "#224AFB",
		},
		{
			f:[{x: 800,y: 0},{x: 600, y: 200},{x: 800, y: 400}],
			color: "#A82F24",
		},
		{
			f:[{x: 400,y: 400},{x: 600, y: 200},{x: 800, y: 400},{x: 600, y: 600}],
			color: "#141D71",
		},
		{
			f:[{x: 400,y: 400},{x: 200, y: 600},{x: 600, y: 600}],
			color: "#F17E4D",
		},
		{
			f:[{x: 200,y:  600},{x: 600, y: 600},{x: 400, y: 800},{x: 0, y: 800}],
			color: "##EC4F93",
		},
		{
			f:[{x: 800,y: 400},{x: 400, y: 800},{x: 800, y: 800}],
			color: "#6CF8FC",
		},
	];
	window.onload = function(){
		var canvas = document.getElementById("canvas");
		var ctx = canvas.getContext("2d");
		canvas.width = 800;
		canvas.height = 800;
		for(var i = 0; i< tangram.length; i++){
			ctx.beginPath();
			ctx.moveTo(tangram[i].f[0].x, tangram[i].f[0].y);
			for (var j= 1; j < tangram[i].f.length; j++){
				ctx.lineTo(tangram[i].f[j].x, tangram[i].f[j].y);
			}
			ctx.closePath();
			ctx.fillStyle = tangram[i].color;
			ctx.fill();
			ctx.lineWidth = 10;
			ctx.stroke();
		}
		//重绘最外层边框
		ctx.lineWidth = 20;
		ctx.strokeRect(0,0,800,800);
	}

	</script>
</body>
</html>
```
一个简单的七巧板就出来啦～
![七巧板.png](http://upload-images.jianshu.io/upload_images/112419-9b8fc1914fa406af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)