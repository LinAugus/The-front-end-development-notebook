# Javascript Math对象和Date对象常用方法详解

## Math对象
Math 是js中的一个内置对象， 它为数学常量和数学函数提供了属性和方法，而不是一个函数对象。
### 属性
- `Math.PI`  
= > 圆周率，一个圆的周长和直径之比，悦等于3.14159.  
- `Math.E`  
= > 欧拉常数，自然对数的底数，约等于2.718.

### 常用的方法
#### 1. Math.abs(x)
＝> 取得x的绝对值

```js
var num = -2;
Math.abs(num); // 2
```
#### 2. Math.random()
= > 返回0到1之间的一个随机数

```js
//随机一个(0,1]的数字
Math.random(); // 0.29510836134846596
//随机返回[0,num)范围内的数字
//Math.random()*num
Math.random*10; //8.028820852140843
//随机返回[start,end)范围内的数字
//Math.random()*(end-start)+start;
Math.random()*(100-50)+50; //71.3784510483645 返回[50,100)的随机数

```
#### 3. Math.floor(x)
= > 返回小于x的最大整数，通俗的讲就是省略小数点后面的值，保留整数，与 `Math.ceil(x)`是相对的。

```js
Math.floor(-1.0);  // -1
Math.floor(2.239424);  // 2
Math.floor(3.9);  // 3
```
#### 4. Math.round(x)
= > 返回四舍五入后的整数

```js
Math.round(1.4);  // 1
Math.round(1.6);  // 2
```
#### 5. Math.sin(x)
= > 返回 x 的正弦值

```js
Math.sin(90*Math.PI/180);  // 1
```
#### 6. Math.cos(x)
= > 返回 x 的余弦值

```js
Math.cos(180*Math.PI/180);  // 1
```
#### 7. Math.ceil(x)
= > 返回x向上取整后的值，通俗的讲就是小数点后有值，就进一位数值

```js
Math.ceil(-1.0);  // -1
Math.ceil(2.239424);  // 3
Math.ceil(3.9);  // 4
```


## Date对象
Date类型是以`UTC`（Coordinated Universal Time，国际协调时间）1970 年1 月1 日午夜（零时）开始经过的毫秒数来保存日期。

### 创建日期的几种方法

```js
new Date();  
new Date(value);  // value为时间戳
new Date(dateString);  // dateString为表示日期的字符串
new Date(year, month[,day[,hour[,minutes[,seconds[,millseconds]]]]]);
//注意：代表月份的整数值是从0（1月）到11（12月）
```

```js
new Date(); // Fri Jul 22 2016 13:32:10 GMT+0800 (CST)
new Date(1469159782236); //  Fri Jul 22 2016 11:56:22 GMT+0800 (CST)
new Date("December 17, 1995 03:24:00");  //Sun Dec 17 1995 03:24:00 GMT+0800 (CST)
new Date("1995-12-17T03:24:00"); //Sun Dec 17 1995 11:24:00 GMT+0800 (CST)
new Date(1995,11,17,3,24,0);  //Sun Dec 17 1995 03:24:00 GMT+0800 (CST)
```

### 常用方法
#### 1. Date.now()
= > 返回自 1970-1-1 00:00:00  UTC (时间标准时间)至今所经过的毫秒数。

```js
Date.now();  //1469166290095
```
#### 2. Date.parse()
= > 解析一个表示日期的字符串，并返回从 1970-1-1 00:00:00 所经过的毫秒数。

```js
Date.parse('Sun Dec 17 1995 03:24:00 GMT+0800 (CST)');
//819141840000
```
#### 3. getFullYear()
= > 返回指定日期对象的年份

```js
var today = new Date();
today.getFullYear();  // 2016
```
#### 4. getMonth()
= > 返回指定日期对象的月份（0-11）

```js
var today = new Date();
today.getMonth();  // 6
```
#### 5. getDate()
= > 返回指定日期对象的月份中的第几天（1-31）

```js
var today = new Date();
today.getDate();  // 22
```
#### 6. getHours()
= > 返回指定日期对象的小时（0-23)

```js
var today = new Date();
today.getHours();  // 14
```
#### 7. getMinutes()
= > 返回指定日期对象的分钟（0-59)

```js
var today = new Date();
today.getMinutes();  // 40
```
#### 8. getSeconds()
= > 返回指定日期对象的秒数（0-59)

```js
var today = new Date();
today.getMinutes();  // 40
```
### 日期格式化
一个简单的日期格式化函数

```js
var format = function (time, format) {
    var t = new Date(time);
    var tf = function (i) {
        return (i < 10 ? '0' : '') + i
    };
    return format.replace(/YYYY|MM|DD|hh|mm|ss/g, function (a) {
        switch (a) {
        case 'YYYY':
            return tf(t.getFullYear());
            break;
        case 'MM':
            return tf(t.getMonth() + 1);
            break;
        case 'mm':
            return tf(t.getMinutes());
            break;
        case 'DD':
            return tf(t.getDate());
            break;
        case 'hh':
            return tf(t.getHours());
            break;
        case 'ss':
            return tf(t.getSeconds());
            break;
        }
    });
}

format(new Date().getTime(),'YYYY-MM-DD hh:mm:ss');
// "2016-07-22 15:08:14"
format(new Date().getTime(),'YYYY年MM月DD日 hh时mm分ss秒');
//"2016年07月22日 15时10分18秒"
```
最后，如果想要格式化日期获得更好的效果，[moment.js](http://momentjs.com) 是个不错的js库。


## 参考资料
[Date](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date#Example:_Several_ways_to_assign_dates)