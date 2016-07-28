# Javascript重温OOP之对象与类的继承

## 对象的继承
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
//实现类似Object.create()的函数
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
从上面可以看出，myCreate实现了将对象p的继承，该方法是由道格拉斯发明的，并被ES5写入标准，`Object.create()`

## 类的继承

