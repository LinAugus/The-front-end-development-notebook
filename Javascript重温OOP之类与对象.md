# Javascript重温OOP之类与对象
## 对象

### 对象的含义
所谓对象，就是一种无序的数据集合，由若干个“键值对”（key-value）构成。

### 对象的创建
1. 使用`new`运算符创建Object
	
	```js
	var p = new Object();
	p.name = "Tony";	
	```
2. 使用对象字面量的形式
	
	```js
	//对象字面量形式
	var p ={
		name: "tony",
		work: function(){
			console.log("working....");
		},
		_age: 18,
		get age(){
			return this._age;
		},
		set age(val){
			if( val <0 || val > 150){
				throw new Error("invalid value");
			}else{
				this._age = val;
			}
		}
	}
	console.log(p.name);
	```

### 对象的基本操作

1. 成员属性的添加
	
	```js
	// Object.defineProperty()方法
	Object.defineProperty(p, "age",{value: 18, writable: false});
	//Object.defineProperties()方法 添加多个属性
	Object.defineProperties(p, {
		salary:{
			value: 1000,
			writable: false
		},
		gender:{
			value: true
		}
	});
	```
2. 成员的遍历

	- 使用 for..in语句
	- `Object.keys()`方法  返回一个包含对象键名的字符串数组
	
		```js
		var o ={};
		o.name = "jack";
		o.age = 20;
		for(var i in o){
			console.log(o[i]);
		} // jack, 20
		Object.keys(o); // ["name", "age"]
		```
		
3. 检查对象是否有某个属性
	
	- `in` 操作符
	- `Object.hasOwnProperty()`方法
		
		```js
		var o = {name: "mariya"}
		"name" in o; // true
		o.hasOwnProperty("name"); // true
		```
		
4. 得到对象的属性特性描述

	- `Object.getOwnPropertyDescriptor(obj,property)`

	```js
	Object.getOwnPropertyDescriptor(o, "name");
	//Object {value: "mariya", writable: true, enumerable: true, configurable: true}
	```	
	
5. 删除属性
	
	- `delete`运算符,但有些对象的属性是删除不了的
	
	```js
	delete o.name; //true
	o.name;  // undefined 
	```
	
### 成员特性

- `configurable` 是否可设置
- `writable` 是否可修改
- `enumerable` 是否可枚举
- `value`属性的值

```js
var person ={};
Object.defineProperties(person,{
	title: {value: 'fe',enumerable: true},
	age: {value: 19, enumerable: true, writable: true}
});
Object.getOwnPropertyDescriptor(person,'title');
// Object {value: "fe", writable: false, enumerable: true, configurable: false}
Object.getOwnPropertyDescriptor(person,'age');
// Object {value: 19, writable: true, enumerable: true, configurable: false}
```

### `Constructor`属性
constructor始终指向创建当前对象的构造函数。

```js
var arr = [];
console.log(arr.constructor === Array); // true
var Foo = function() {};
console.log(Foo.constructor === Function); // true
// 由构造函数实例化一个obj对象
var obj = new Foo();
console.log(obj.constructor === Foo); // true
console.log(obj.constructor.constructor === Function); // true
```

每个函数都有一个默认的属性`prototype`，而这个`prototype`的`constructor`默认指向这个函数。

```js
function Person(name) {
    this.name = name;
};
Person.prototype.getName = function() {
    return this.name;
};
var p = new Person("jack");
console.log(p.constructor === Person);  // true
console.log(Person.prototype.constructor === Person); // true
console.log(p.constructor.prototype.constructor === Person); // true
```

## 类的创建
虽然js是门基于对象的语言，但是没有类这一概念的，虽然保留了`class`的关键字，但在ES6之前是无法使用的。所以，可以用构造函数模拟类的创建，也就是伪类。

所谓"构造函数"，其实就是一个普通函数，但是内部使用了`this`变量。对构造函数使用`new`运算符，就能生成实例，并且`this`变量会绑定在实例对象上。

每一个构造函数都有一个`prototype`属性，指向另一个对象。这个对象的所有属性和方法，都会被构造函数的实例继承。

这意味着，我们可以把那些不变的属性和方法，直接定义在`prototype`对象上。

```js
//构造函数模式
function Person(age, name){ //Class
	this.age = age;
	this.name = name;
}
//将公共的属性或方法放在prototype属性上
Person.prototype.headCount = 1;
//创建实例对象
var p = new Person(19, 'johnsom');
var p1 = new Person(20, 'allen');
```

## this
`this`表示当前对象，如果在全局作用范围内使用`this`，则指代当前页面对象window； 如果在函数中使用`this`，则`this`指代什么是根据运行时此函数在什么对象上被调用。 我们还可以使用`apply`和`call`两个全局方法来改变函数中`this`的具体指向。

### 1. 全局代码中的this

```js
console.log(this === window); //true 全局范围内使用this指向window对象
```

### 2. 普通的函数调用


```js
function f(){
	this.name = "tony"; // this在运行时指向window对象,在严格模式下则是undefined
}
```

### 3. 在对象中使用

```js
var o = {
	name: "tony",
	print: function(){
		console.log(this.name);  //this指向对象o，但是可以改变其指向
	}
};
```

### 4. 作为构造函数

```js
new F(); // 函数内部的this指向新创建的对象。
```

### 5. 多层嵌套的内部函数

```js
var name = "global";
var person = {
	name : "person",
	hello : function(sth){
		var sayhello = function(sth) {
			console.log(this.name + " says " + sth);
		};
		sayhello(sth);
	}
}
person.hello("hello world");//global says hello world
```
在内部函数中，this没有按预想的绑定到外层函数对象上，而是绑定到了全局对象。这里普遍被认为是JavaScript语言的设计错误，因为没有人想让内部函数中的this指向全局对象。一般的处理方式是将this作为变量保存下来，一般约定为that或者self：

```js
var name = "global";
var person = {
	name : "person",
	hello : function(sth){
		var that = this;
		var sayhello = function(sth) {
			console.log(that.name + " says " + sth);
		};
		sayhello(sth);
	}
}
person.hello("hello world");//person says hello world
```

### 6. 事件中的this

```js
var ele = document.getElementById("id");
ele.addEventListener('click',function(){
	console.log(this);  //this指向dom元素
});
```

### 7. 使用apply和call改变this的指向
apply和call类似，只是后面的参数是通过一个数组传入，而不是分开传入。两者都是将某个函数绑定到某个具体对象上使用，自然此时的this会被显式的设置为第一个参数。两者的方法定义：

```js
call( thisArg [，arg1，arg2，… ] );  // 参数列表，arg1，arg2，...
apply(thisArg [，argArray] );     // 参数数组，argArray
```

```js
var name = 'global';
var o = {
	name: 'job',
	getName: function(){
		console.log(this.name);
	}
};
o.getName(); // job

//用call或apply改变函数中this的指向
o.getName.call(this); // global

```
简单的总结：

1. 当函数作为对象的方法调用时，this指向该对象。
2. 当函数作为淡出函数调用时，this指向全局对象（严格模式时，为undefined）
3. 构造函数中的this指向新创建的对象
4. 嵌套函数中的this不会继承上层函数的this，如果需要，可以用一个变量保存上层函数的this。

### 8. `bind（）`
一个问题：

```js
$("#ele").click = obj.handler;
```
如果在handler中用了this，this会绑定在obj上么？显然不是，赋值以后，函数是在回调中执行的，this会绑定到$(“#some-div”)元素上。那我们如何能解决回调函数绑定的问题？ES5中引入了一个新的方法，bind():

```js
fun.bind(thisArg[, arg1[, arg2[, ...]]])

thisArg
当绑定函数被调用时,该参数会作为原函数运行时的this指向.当使用new 操作符调用绑定函数时,该参数无效.
arg1, arg2, ...
当绑定函数被调用时,这些参数加上绑定函数本身的参数会按照顺序作为原函数运行时的参数.
```
该方法创建一个新函数，称为绑定函数，绑定函数会以创建它时传入bind方法的第一个参数作为this，传入bind方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数.

```js
$("#ele").click(person.hello.bind(person));
//相应元素被点击时，输出person says hello world
```
该方法也很容易模拟，看下Prototype.js中bind方法的源码：

```js
Function.prototype.bind = function(){
  var fn = this, args = Array.prototype.slice.call(arguments), object = args.shift();
  return function(){
    return fn.apply(object,
      args.concat(Array.prototype.slice.call(arguments)));
  };
};
```

## 参考资料
[js老生常谈之this,constructor ,prototype](http://www.haorooms.com/post/js_constructor_pro)

[详解JavaScript中的this](http://blog.jobbole.com/39305/)