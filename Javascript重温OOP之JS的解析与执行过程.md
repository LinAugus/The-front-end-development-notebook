# Javascript重温OOP之JS的解析与执行过程
了解js面向对象编程之前，首先要了解js的执行顺序。js的解析过程分为两个阶段：预处理阶段与执行期。

## 预处理阶段
在预处理阶段，js会首先创建一个执行上下文对象（Execute Context,然后扫描声明式函数和用`var`定义的变量，将其加入执行上下文环境中，看下面栗子：

```js
var a = 5; 
b = 1;
function f(){}
var g = function(){}

//执行上下文对象
/*Execute Context{
	a: undefined
	f: 对函数的引用
}*/
```
从上面可以看出，js在预处理阶段创建了一个预处理对象，将声明式函数和`var`定义的变量放入其中，这里忽略了没有用`var`声明的b以及函数表达式 g。

```js
alert(a);  // undefined
alert(b);	// 报错 b in not defined
alert(c);	// 函数体被打印出来
alert(d);	// 报错

var a = 1;
b = 5;
function c(){
	console.log('c');
}
var d = function(){
	console.log('d');

}
```

从上面例子的输出结果就可以看出js预处理阶段做了哪些事情，对于没有加进函数变量预处理阶段的变量或函数，会直接报错。

对于冲突解决有两种情况：

- 处理函数声明有冲突时，会覆盖前面
- 处理函数变量声明有冲突时，会直接忽略


```js
alert(f); // 弹出function f(){console.log('fff');}
function f(){
	console.log('ff');
}

var f = 10;

function f(){
	console.log('fff');
}
```
总结一句话：函数是js里的第一等公民。


## 执行阶段
在执行阶段中，js会先扫描函数声明后扫描变量，然后给预处理阶段中执行上下文对象中的成员赋值，如果没有用`var`声明的变量，会成为外部执行上下文的成员。

```js
alert(a);
alert(f);
var a = 1;
function f(){}

//在执行阶段的执行上下文对象
/*{
	a: 1; // 由undefined赋值为1
	f: 指向对应函数
}*/
```
