# Javascript重温OOP之面向对象
一般面向对象包含：继承，封装，多态，抽象

## 对象形式的继承
### 浅拷贝

```js
var Person = {
	name: 'allin',
	age: 18,
	address: {
		home: 'home',
		office: 'office',
	}
	sclools: ['x','z'],
};

var programer = {
	language: 'js',
};

function extend(p, c){
	var c = c || {};
	for( var prop in p){
		c[prop] = p[prop];
	}
}
extend(Person, programer);
programer.name;  // allin
programer.address.home;  // home
programer.address.home = 'house';  //house
Person.address.home;  // house
```

从上面的结果看出，浅拷贝的缺陷在于修改了子对象中引用类型的值，会影响到父对象中的值，因为在浅拷贝中对引用类型的拷贝只是拷贝了地址，指向了内存中同一个副本。

### 深拷贝

```js
function extendDeeply(p, c){
	var c = c || {};
	for (var prop in p){
		if(typeof p[prop] === "object"){
			c[prop] = (p[prop].constructor === Array)?[]:{};
			extendDeeply(p[prop], c[prop]);
		}else{
			c[prop] = p[prop];
		}
	}
}
```

利用递归进行深拷贝，这样子对象的修改就不会影响到父对象。

```js
extendDeeply(Person, programer);
programer.address.home = 'allin';
Person.address.home; // home
```

### 利用call和apply继承

```js
function Parent(){
	this.name = "abc";
	this.address = {home: "home"};
}
function Child(){
	Parent.call(this);
	this.language = "js"; 
}
```
### ES5中的Object.create()

```js
var p = { name : 'allin'};
var obj = Object.create(o);
obj.name; // allin
```
`Object.create()`作为`new`操作符的替代方案是ES5之后才出来的。我们也可以自己模拟该方法：

```js
//模拟Object.create()方法
function myCreate(o){
	function F(){};
	F.prototype = o;
	o = new F();
	return o;
}
var p = { name : 'allin'};
var obj = myCreate(o);
obj.name; // allin
```
目前，各大浏览器的最新版本（包括IE9）都部署了这个方法。如果遇到老式浏览器，可以用下面的代码自行部署。

```js
　　if (!Object.create) {
　　　　Object.create = function (o) {
　　　　　　 function F() {}
　　　　　　F.prototype = o;
　　　　　　return new F();
　　　　};
　　}
```


## 类的继承
### Object.create()

```js
function Person(name, age){}
Person.prototype.headCount = 1;
Person.prototype.eat = function(){
	console.log('eating...');
}
function Programmer(name, age, title){}

Programmer.prototype = Object.create(Person.prototype); //建立继承关系
Programmer.prototype.constructor = Programmer;  // 修改constructor的指向
```

## 封装
### 命名空间
js是没有命名空间的，因此可以用对象模拟。

```js
var app = {};  // 用对象模拟命名空间
app.module1 = {}; //模块1
app.module1.method = function(){
	console.log('module1 method');
};
app.module1.method();  //module1 method
```
### 静态成员

```js
var staticdemo = function(){
	// 静态方法
	var pname = 'allin';
	var work = function(){
		console.log( name+ 'is working..');
	};
};
staticdemo.pname; // undefined
staticdemo.work(); // demo.work is not a function(…)
```
