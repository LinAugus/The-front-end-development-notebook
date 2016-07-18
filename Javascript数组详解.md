# Javascript数组详解

## 数组的定义
数组是按序号排列的一组值，每个值的位置都有编号（从0开始）。数组本质上是一种特殊的对象。它的键名是按（0，1，2...）排列的一组数字。

1. 创建数组：

	```javascript
	var arr = new Array(values);
	var arr = [vaules];
	```
2. 判断比是否是个数组
	- Array.isArray(arr)
	- arr instanceof Array  
3. 取数组元素  
	arr[index]
4. `length` 属性  
	返回数组的成员数量。
	Javascript使用一个32位整数，保存数组的元素个数。这意味着数组的成员最多只有4294967295个（2<sup>32</sup>-1）。
	
	```javascript
	var arr = ['a','b'];
	arr.length  // 2
	arr.[10] = 'c'
	arr.length //11
	```
	从上面的输出结果可以看出，`length`属性可以动态变化，如果将length属性设为 0 ，就会将数组清空。如果设置的数小于原有的个数，那么在这个数后面的数值就自动删除了。反过来，如果设置的数大于原有的个数，数组的成员将增大，都为 `undefined`。

	```javascript
	var a = [1,2];
	a.length = 5;
	a[4] // undefined
	a.length = 1;
	a.[1] // undefined
	a.length = 0;
	a  //[]
	```

5. 增加数组元素
	- `push()`方法 在数组的末尾增加一个或多个元素，并返回数组的新长度。
	- `unshift()`方法 在数组的开头增加一个或多个元素，并返回数组的新长度。
	- `length` 属性

	```javascript
	var arr = [1, 2, 3]
	arr.push(4)
	arr  // 1, 2, 3, 4
	arr.unshift(6)
	arr  // 6, 1, 2, 3, 4
	arr[arr.length] = 7  // 与push()方法类似
	arr  // 6, 1, 2, 3, 4, 7
	
	```
6. 删除数组中的元素  
	- `delete` 运算符，可以删除数组中的某个元素，但这不会改变`length`属性的值.
	- `pop()` 方法 删除数组的最后一个元素，并返回这个元素。
	- `shift()` 方法 删除数组的第一个元素，并返回这个元素。
	
	```javascript
	var arr = [1,2,3];
	delete arr[0];
	arr   // [undefined,2,3]
	arr.length  // 3
	var last = arr.pop()
	var first = arr.shift()
	last // 3
	first // undefined
	arr //2
	```

## 类数组对象
在js中，有些对象被叫做“类数组对象”（array-like object），因为这些对象看起来很像数组，可以使用`length`属性，但是无法使用数组的方法。  
典型的类数组对象是函数的`arguments`对象，以及大多数DOM元素集，还有字符串。

```javascript
// arguments对象
function args() {return arguments; }
var arraylike = args('a','b')
arrayLike[0]  // 'a'
arrayLike.length // 2
arrayLike instanceof Array // false
Array.isArray(arrayLike)  // false

// DOM元素集
var elts = document.getElementsByTagName('p');
elts.length  // 3
eles instanceof Array  // false

//字符串
'abc'[1]  // 'b'
'abc'.length  // 3
'abc' instanceof Array  // false
```

数组的`slice`方法能将类数组对象，变成真正的数组。`slice`方法后面会结介绍。

```javascript
var arr = Array.prototype.slice.call(arrayLike);
```

## 数组的遍历
1. for...in 循环

	```javascript
	var a =[1, 2, 3];
	a.other = 'other';
	for (var i in arr){
		console.log( arr[i]);
	}
	// 1, 2, 3, other
	```
	从上面的输出结果可以看出，利用for..in循环会将动态添加的非数字键的值遍历出来，因此需要使用的时候需要注意。
	
2. for 循环和 while 循环

	```javascript
	var a = [1, 2, 3];
	
	// for循环
	for(var i = 0; i < a.length; i++) {
	  console.log(a[i]);
	}
	
	// while循环
	var i = 0;
	while (i < a.length) {
	  console.log(a[i]);
	  i++;
	}
	
	var l = a.length;
	while (l--) {
	  console.log(a[l]);
	}
	```
	
3. `forEach()`方法
	
	```javascript
	//array.forEach(callback[, thisArg])
	//callback 在数组的每一项上执行的函数，接受三个参数：item: 数组当前项的值，index: 当前项的索引，arr:数组本身。
	var arr = [1, 2, 3]
	arr.forEach(function(item, index, arr){
		console.log(item, index);
	});
	//1 0
	//2 1
	//3 2
	```

## 数组常用的方法
1. join() 将数值转换为字符串

	```javascript
	var arr = [1, 2, 3];
	arr.join(); // "1,2,3"
	arr.join("_"); // "1_2_3"
	```
2. reverse() 将数组逆序

	```javascript
	// 原数组会被修改
	var arr = [1, 2, 3];
	arr.reverse(); // [3, 2, 1]
	arr; // [3, 2, 1]
	```
3. sort() 数组排序  
	默认情况下是升序排列的，底层是调用了每个数组项的 toString() 方法，然后比较得到字符串，即使每个数组项的数值是数字，比较的也是字符串。
	
	```javascript
	// 原数组会被修改
	var arr = [1, 12, 213, 1432, 'a'];
	arr.sort(); // [1, 12, 1432, 213, "a"]
	arr.sort(function(a, b){
		return b-a; //按倒序排列数组
	});
	
	```
4. concat() 数组合并  
	用法：array.concat(value1, value2, ..., valueN)
	
	```javascript
	// 原数组不会被修改
	var arr =[1, 2, 3]
	arr.concat(4, 5);
	arr; //[1, 2, 3]
	arr.concat([11,2],3); // [1, 2, 3, 11, 2, 3]
	```
5. slice() 返回部分数组  
	用法：array.slice(begin[,end])
	
	slice用于复制数组，复制完后旧数组不变，返回得到的新数组是旧数组的子集。
	
	第一个参数begin是开始复制的位置，需要注意的是，可以设负数。设负数表示从尾往前数几个位置开始复制。例如slice(-2)将从倒数第2个元素开始复制。另外需要注意的是，该参数虽未标注为可选，但实际上是可以省略的，省略的话默认为0。
	
	第二个参数end可选，表示复制到该位置的前一个元素。例如slice(0,3)将得到前3个元素，但不包含第4个元素。不设的话默认复制到数组尾，即等于array.length。
	
	```javascript
	//原数组不会被修改
	var arr = [1, 2, 3, 4, 5];
	arr.slice(); //[1, 2, 3, 4, 5]
	arr.slice(1,3); // [2, 3]
	arr.slice(1, -1); // [2, 3, 4]
	arr; // [1, 2, 3, 4, 5]
	```
6. splice() 数组拼接  
	用法：array.splice(start, deleteCount[, item1[, item2[, ...]]])
	- start 指要从哪一位开始修改内容，如果超出了数组的长度，则从数组末尾开始添加内容；如果是负值，则表示从数组末位开始的第几位。
	- deleteCount 表示要移除的数组元素的个数
	- item 要添加进数组的元素
	
	最主要的的用途是向数组的中部插入元素。
	
	```javascript
	//原数组会被修改
	var arr = [1, 2, 3, 4, 5];
	//从第三个数组元素删除
	arr.splice(2); // returns [3, 4, 5] 
	arr; // [1, 2]
	//从第三个数组元素删除，删除两个元素
	arr.splice(2, 2) // returns [3, 4]
	arr; // [1, 2, 5]
	//将'a','b'替换到数组的第二个元素
	arr.splice(1, 1, 'a', 'b')
	
	```
7. forEach() 数组遍历  
	用法：array.forEach(callback[, thisArg])  
	callback 在数组的每一项上执行的函数，接受三个参数：item：数组当前项的值，index: 当前项的索引，arr：数组本身。
	
	```javascript
	var arr = [1, 2, 3]
	arr.forEach(function(item, index, arr){
		console.log(item, index);
	});
	//1 0
	//2 1
	//3 2
	```
8. map() 数组映射  
	map映射创建新数组，用法： map(function(value, index, array) { … }, [thisArg] );。和forEach一样，不赘述。唯一需要注意的的是回调函数需要有return值，否则新数组都是undefined。
	
	其实map能干的事forEach都能干，你可以把map理解为forEach的一个特例，专门用于：通过现有的数组建立新数组。

	```javascript
	//原数组未被修改
	var arr= [1, 2, 3];
	arr.map(function(x){
		return x+10;  // 需要 return 值，否则新数组里都是 undefined
	});  // [11, 12, 13]
	arr; // [1, 2, 3]
	```
9. filter() 数组过滤  
	filter用于过滤数组，用法： filter( function(value, index, array) { … }, [thisArg] ); 。唯一需要注意的的是回调函数需要return布尔值true或false，
	
	```javascript
	//原数组未被修改
	var arr= [1, 2, 3, 4, 5, 6, 7, 8];
	arr.filter(function(x,index){
		return x%2 == 0;
	}); // [2, 4, 6, 8]
	arr; // [1, 2, 3, 4, 5, 6, 7, 8]
	```
10. every() some() 数组判断  
	some表示只要某一个满足条件就OK，every表示全部满足条件才OK。  
	用法：
	
	- arr.every(callback[, thisArg])  
	- arr.some(callback[,thisArg])

	```javascript
	var arr= [1, 2, 3, 4, 5];
	arr.every(function(x){
		return x < 6;
	}); // true
	arr.every(function(x){
		return x > 6;
	}); // false
	arr.some(function(x){
		return x === 1;
	});  // true
	arr.some(function(x){
		return x === 6;
	}); // false
	```
11. reduce() reduceRight()  
	reduce() 方法接收一个函数作为累加器（accumulator），数组中的每个值（从左到右）开始合并，最终为一个值。  两者都是用于迭代运算。区别是reduce从头开始迭代，reduceRight从尾开始迭代。

	用法： reduce( function(previousValue, currentValue, currentIndex, array) { … }, [initialValue] );

	第一个参数是回调函数，有4个参数：previousValue，currentValue，currentIndex，array。看名字也能知道意思：前一个值，当前值，当前索引，数组本身。

	第二个参数initialValue可选，表示初始值。如果省略，初始值为数组的第一个元素，这样的话回调函数里previousValue就是第一个元素，currentValue是第二个元素。因此不设initialValue的话，会少一次迭代。
	
	```javascript
	//将数组所有项相加
	var arr = [0, 1, 2, 3];	var sum = arr.reduce(function(a, b) {     return a + b	}, 0); // 6	arr; //[0, 1, 2, 3]
	//数组扁平化
	var flattened = [[0, 1], [2, 3], [4, 5]];
	flattened.reduce(function(a,b){
		return a.concat(b);
	}); // returns [0, 1, 2, 3, 4, 5]
	```
	
12. indexOf() lastIndexOf() 数组检索  
	用法：indexOf( searchElement, [fromIndex = 0]) ／ lastIndexOf( searchElement , [fromIndex = arr.length – 1])  
	第一个参数searchElement即需要查找的元素。第二个参数fromIndex可选，指定开始查找的位置。如果忽略，indexOf默认是0，lastIndexOf默认是数组尾。  
	两者都用于返回项目的索引值。区别是indexOf从头开始找，lastIndexOf从尾开始找。如果查找失败，无匹配，返回-1
	
	```javascript
	var arr = ['a', 'b', 'c', 'd', 'e'];
	arr.indexOf('c');  // 2 找到返回数组下标
	arr.indexOf('c', 3); // -1 指定从3号位开始查找
	arr.indexOf('f'); // -1 没找到该元素
	arr.lastIndexOf('c'); // 2
	arr.lastIndexOf('c',2); // 2
	arr.lastIndexOf('f'); // -1 没找到该元素
	```
13. isArray() 判断是否是数组  
	用法：Array.isArray(value)
	
	```javascript
	var arr = [];
	var a = "not array";
	Array.isArray(arr); // true
	Array.isArray(a); // false
	```


## 参考资料
[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/)