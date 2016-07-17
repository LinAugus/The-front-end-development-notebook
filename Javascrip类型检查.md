# Javascript 类型检查
在 Javascript 中常见的类型检查手段主要有：`typeof`,`instanceof`,`constructor`和`toString`。

## typeof 运算符
`typeof`操作符返回的是类型字符串.

```javascript
typeof 12   // "number"
typeof "hello" // "string"
typeof true   // "boolean"
typeof function(){}   // "function"
typeof undefined   // "undefined"
typeof [1,2] // "object"
typeof {}   // "object"
typeof NaN // "number"
typeof null // "object"
```
从上面的输出结果可以得出结论：`typeof`只能用来检测基本数据类型。在实际的生产环境中，`typeof`常常用来判断变量是否为`undefined`。

## instanceof 运算符
在 JavaScript 中，判断一个变量的类型尝尝会用 typeof 运算符，在使用 typeof 运算符时采用引用类型存储值会出现一个问题，无论引用的是什么类型的对象，它都返回 “object”。ECMAScript 引入了另一个 Java 运算符 instanceof 来解决这个问题。instanceof 运算符与 typeof 运算符相似，用于识别正在处理的对象的类型。与 typeof 方法不同的是，instanceof 方法要求开发者明确地确认对象为某特定类型。

`instanceof`操作符用于检测某个对象的原型链是否包含某个构造函数的prototype属性。

```javascript
123 instanceof Number               //false
"123" instanceof String             //false
true instanceof Boolean,            //false
[] instanceof Array                 //true
{} instanceof Object                //true
(function(){}) instanceof Function  //true
undefined instanceof Object         //true
null instanceof Object              //true
new Date() instanceof Date          //true
new RegExp() instanceof RegExp      //true
new Error() instanceof Error        //true
```

从输出结果来看, `undefined`和`null`是检测不成`Object`类型的，要使用 `instanceof` 进行变量检测时，需要首先判断是否是 `undefined` 和 `null`

## constructor属性

在使用`instanceof`检测变量类型时，我们检测不到`number`, `‘string’`, `bool`的类型的。因此，我们需要换一种方式来解决这个问题。

constructor本来是原型对象上的属性，指向构造函数。但是根据实例对象寻找属性的顺序，若实例对象上没有实例属性或方法时，就去原型链上寻找，因此，实例对象也是能使用constructor属性的。但是`undefinded` 和 `null`是没有 `constructor` 属性的。

```javascript
function Person(){}
var Tom = new Person();
console.log(Tom.constructor == Person);  //true
(123).constructor == Number;  //true
("hello").constructor == String ;  //true
(true).constructor == Boolean;  //true
[].constructor == Array;  //true
{}.constructor == Object;  //true
(function(){}).constructor == Function  //true
```
从上面的输出结果可以看出，除了`undefined` 和 `null`，其他类型的变量均能使用`constructor`判断出来。

但是`constructor`也不是万无一失的，它是能被修改的，会导致输出的结果不正确。例如：

```javascript
function Person(){}
function Student(){}
Student.prototype = new Person();
var Jack = new Student();
console.log( Jack.constructor == Student ); //false
console.log( Jack.constructor == Person ); //true
``` 
在上面的例子中， Student原型中的`constructor`属性被修改指向到了Person，导致检测不出实例对象Jack真实的构造函数。


## Object.prototype.toString
Object.prototype.toString.call(变量)输出的是一个字符串，字符串里有一个数组，第一个参数是Object，第二个参数就是这个变量的类型，而且，所有变量的类型都检测出来了，我们只需要取出第二个参数即可。或者可以使用Object.prototype.toString.call(arr)=="object Array"来检测变量arr是不是数组。

```javascript
Object.prototype.toString.call(123);
//"[object Number]"
Object.prototype.toString.call("hello");
//"[object String]"
Object.prototype.toString.call(false);
//"[object Boolean]"
Object.prototype.toString.call([]);
//"[object Array]"
Object.prototype.toString.call({});
//"[object Object]"
Object.prototype.toString.call(function(){});
//"[object Function]"
Object.prototype.toString.call(null)
//"[object Null]"
Object.prototype.toString.call(undefined)
//"[object Undefined]"
```
在chrome运行的效果：

ECMA里是这样定义`Object.prototype.toString.call`的：

> Object.prototype.toString( ) When the toString method is called, the following steps are taken:   
1. Get the [[Class]] property of this object.   
2. Compute a string value by concatenating the three strings “[object “, Result (1), and “]”.   
3. Return Result (2)

上面的规范定义了`Object.prototype.toString`的行为：首先，取得对象的一个内部属性[[Class]]，然后依据这个属性，返回一个类似于”[object Array]”的字符串作为结果（看过ECMA标准的应该都知道，[[]]用来表示语言内部用到的、外部不可直接访问的属性，称为“内部属性”）。利用这个方法，再配合call，我们可以取得任何对象的内部属性[[Class]]，然后把类型检测转化为字符串比较，以达到我们的目的。






