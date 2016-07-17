## 在这之前
需要你懂得以下方法的使用：

- beginPath()
- moveTo()
- lineTo()
- closePath()

具体用法可以参考我的上一篇文章 [canvas入门－利用canvas制作一个七巧板](http://www.jianshu.com/p/5702f709ca7b)

## 矩形的绘制
canvas提供了三种绘制矩形的方法：
	
- fillRect(x, y, width, height)
	绘制一个填充的矩形
- strokeRect(x, y, width, height)
	绘制一个矩形的边框
- clearRect(x, y, width, height)
	清除制定矩形区域啊

上面方法的参数里 x 和 y 分别为相对于canvas原点的坐标，width 和 height 设置了矩形的尺寸。
举个栗子：  

```
window.onload = function(){
	var canvas = document.getElementById("canvas");
	var ctx = canvas.getContext("2d");
	canvas.width = 900;
	canvas.height = 600;
	//绘制矩形
	ctx.fillRect(0,0,300,300);
	ctx.strokeRect(5,200,200,200);
	ctx.clearRect(0,0,100,100);
};
```
上面代码先取得canvas对象上下文，接着绘制了一个填充矩形和边框矩形，并清除了一个矩形区域。

## 绘制五角星之前
在这之前，需要分析五角星各个顶点的位置，以及弧度，由于数学水平有限，都怪当初不好好学 T.T
![star.jpg](http://upload-images.jianshu.io/upload_images/112419-856ac3106dcc478b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- x:cos(18+i\*72) \* r //确定 x 坐标值
- y:-sin(54+i\*72) \* r //确定 y 坐标值
- 角度化 num/180*Math.PI //js将数值角度化的转换公式
- 400是圆的圆心点，如果不加400，则圆心点为0,0

下面为绘制五角星的函数，有五个参数：ctx:绘图环境，R:大圆半径，x：x坐标值， y：y坐标值, rot：旋转角度

```
function drawStar(ctx,R,x,y,rot){
	ctx.beginPath();
	for (var i = 0; i < 5; i++ ) {
		ctx.lineTo(Math.cos((18+i*72-rot)/180*Math.PI)*R + x,-Math.sin((18+i*72-rot)/180*Math.PI)*R + y);
		ctx.lineTo(Math.cos((54+i*72-rot)/180*Math.PI)*R/2.4 + x,-Math.sin((54+i*72-rot)/180*Math.PI)*R/2.4 + y);
	};
	ctx.closePath();
	ctx.fill();
}
```

下面贴上完整代码：

```
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<title>canvas</title>
</head>
<body>
	<canvas id="canvas" style="border: 1px solid #aaa; display: block; margin: 50px auto">
		当前浏览器不支持cnavas		
	</canvas>
<script type="text/javascript">
window.onload = function(){
	var canvas = document.getElementById("canvas");
	var ctx = canvas.getContext("2d");
	//绘制中国国旗
	ctx.fillStyle = "red";
	ctx.fillRect(0,0,900,600);
	//绘制五角星
	//x:cos(18+i*72)*r
	//y:-sin(54+i*72)*r
	//角度化 num/180*Math.PI
	//依次制定大圆和小圆的一个点
	//400是圆的圆心点，如果不加400，则圆心点为0,0
	ctx.fillStyle = "yellow";
	drawStar(ctx, 60, 120, 160, 0);
	var starF = [35, 5, 335, 305];
	for (var j = 0; j < starF.length; j++){
		var rot = 18 + j * 15;
		var x = Math.cos( starF[j]/180 * Math.PI ) * 180 + 120;
		var y = -Math.sin( starF[j]/180 * Math.PI ) * 180 + 160;
		drawStar(ctx, 60/2.4, x, y, rot);
	}
};
function drawStar(ctx,R,x,y,rot){
	ctx.beginPath();
	for (var i = 0; i < 5; i++ ) {
		ctx.lineTo(Math.cos((18+i*72-rot)/180*Math.PI)*R + x,-Math.sin((18+i*72-rot)/180*Math.PI)*R + y);
		ctx.lineTo(Math.cos((54+i*72-rot)/180*Math.PI)*R/2.4 + x,-Math.sin((54+i*72-rot)/180*Math.PI)*R/2.4 + y);
	};
	ctx.closePath();
	ctx.fill();
}
</script>
</body>
</html>
```

最后效果如下：
![红旗.png](http://upload-images.jianshu.io/upload_images/112419-744fac465a5396d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)