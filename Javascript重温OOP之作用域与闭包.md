# Javascript重温OOP之作用域与闭包

## 作用域
### 定义
>  在编程语言中，作用域控制着变量与参数的可见性及生命周期，它能减少名称冲突，而且提供了自动内存管理  --javascript 语言精粹

我理解的是，一个变量、函数或者成员可以在代码中访问到的范围。

- js的变量作用域是基于其特有的作用域链的。
- 全局变量都是window对象的属性
- 没有块级作用域
- 函数中声明的变量在整个函数中都有定义。

```js
//全局作用域
var a = 10;

//没有块级作用域
if(fasle){
	var b =2;
}

//函数作用域
function f(){
	var a = 1;
	console.log(a);
}
```
### 作用域链
> 作用域链是一个对象列表，上下文代码中出现的标识符在这个列表中进行查找。

如果一个变量在函数自身的作用域（在自身的变量/活动对象）中没有找到，那么将会查找它父函数（外层函数）的变量对象，以此类推。

举个栗子：

```js
function f (){
	var x =100;
	function g(){
		console.log(x);
	}
	g();
}
f();
```
这里形成了一条作用域链，当函数g访问不到变量时，它会通过内部的[[scope]]对象查找作用域链上的执行上下文，当找到就终止，找不到会继续，直到window对象上也没有的时候，会报错。

需要注意的是，用`new Function`创建的函数，其作用域指向全局作用域。

```js
function f(){
	var x = 100;
	// g.[[scope]] == window
	var g = new Function("", "alert(x)");

	g();
}
f();
//报错 x is not defined
```


## 闭包

### 闭包的含义
> 在计算机科学中，闭包（英语：Closure），又称词法闭包（Lexical Closure）或函数闭包（function closures），是引用了自由变量的函数。这个被引用的自由变量将和这个函数一同存在，即使已经离开了创造它的环境也不例外。所以，有另一种说法认为闭包是由函数和与其相关的引用环境组合而成的实体。--维基百科

我的理解就是，一个函数捕获其父函数的自由变量，这就形成了闭包。

栗子：

```js
function f1(){
	var a = 10;
	var b = 20;
	return function f2(){
		console.log(a);
	}
}
var result = f1();
result();
```

### 闭包的本质
作用域链的存在

### 闭包的好处

- 减少全局变量
- 减少传递给函数的参数数量
- 封装

### 使用闭包的注意点

- 对捕获的变量只是个引用，不是复制
	
	```js
	function f(){
		var num = 1;
		function g(){
			alert(num);
		}
	
		num++;
		g(); //2
	}
	f();
	```
- 父函数每调用一次，会产生不同的闭包
	
	```js
	function f(){
		var num = 1;
		return function(){
			num++;
			alert(num);
		}
	}
	var result1 =f();
	result1();
	result1();
	var result2 =f();
	result2();
	result2();
	//两次都弹出一样的数值
	```
- 循环中出现的问题

	```js
	for (var i = 1; i <= 3; i++) {
		var ele = document.getElementById(i);
		ele.onclick = function(){
			alert(i);
		}
	};
	// i为4
	```
	解决方法：
	
	```js
	for (var i = 1; i <= 3; i++) {
	var ele = document.getElementById(i);
		ele.onclick = (function(id){
			return function(){
			alert(id);
		}})(i);
	};
	// 1，2，3
	```