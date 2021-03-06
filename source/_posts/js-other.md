---
title: Javascript零散笔记
date: 2017-08-06 22:30:57
tags: [js]
categories: 
	- Javascript
---


## JavaScript 中包含以下 7 个全局函数
escape( )、eval( )、isFinite( )、isNaN( )、parseFloat( )、parseInt( )、unescape( )。

## JavaScript为指定元素绑定一个事件处理器函数

## Javascript块内声明函数
不要在块内声明一个函数（严格模式会报语法错误）。如果确实需要在块中定义函数，可以使用函数表达式来声明函数。

	```
	/* Recommended */
	if (x) {
		var foo = function() {};
	}


	/* Wrong */
	if (x) {
		function foo() {}
	}
	```
<!-- more -->
## Jquery获取宽高
```
alert($(window).height()); //浏览器当前窗口可视区域高度 
alert($(document).height()); //浏览器当前窗口文档的高度 
alert($(document.body).height());//浏览器当前窗口文档body的高度 
alert($(document.body).outerHeight(true));//浏览器当前窗口文档body的总高度 包括border padding margin 
alert($(window).width()); //浏览器当前窗口可视区域宽度 
alert($(document).width());//浏览器当前窗口文档对象宽度 
alert($(document.body).width());//浏览器当前窗口文档body的高度 
alert($(document.body).outerWidth(true));//浏览器当前窗口文档body的总宽度 包括border padding margin 
```
## 浏览器兼容性问题
- SD9017: Firefox 不支持 DOM 对象的 outerHTML、innerText、outerText 属性(参见<http://w3help.org/zh-cn/causes/SD9017>)
- SD9010: 仅 IE 中的 createElement 方法支持传入 HTML String 做参数
- SD9006: IE 混淆了 DOM 对象属性（property）及 HTML 标签属性（attribute），造成了对 setAttribute、getAttribute 的不正确实现

_____________
2017.08.15更新
_____________

## parseInt()
parseInt() 函数解析一个字符串参数，并返回一个指定基数的整数 (数学系统的基础)。

### 语法
```
parseInt(string, radix);
```
### 参数
`string`
要被解析的值。如果参数不是一个字符串，则将其转换为字符串(使用`ToString`抽象操作)。字符串开头的空白符将会被忽略。
`radix`
一个介于2和36之间的整数(数学系统的基础)，表示上述字符串的基数。比如参数"10"表示使用我们通常使用的十进制数值系统。始终指定此参数可以消除阅读该代码时的困惑并且保证转换结果可预测。当未指定基数时，不同的实现会产生不同的结果，通常将值默认为10。

### 返回值
返回解析后的整数值。 如果被解析参数的第一个字符无法被转化成数值类型，则返回`NaN`。

### 描述
`parseInt`函数将其第一个参数转换为字符串，解析它，并返回一个整数或`NaN`。如果不是`NaN`，返回的值将是作为指定基数（基数）中的数字的第一个参数的整数。
例如：`radix`参数为10将会把第一个参数看作是一个数的十进制表示，8对应八进制，16对应十六进制，等等。基数大于10时，用字母表中的字母来表示大于9的数字。例如十六进制中，使用A到F。
如果`parseInt`遇到了不属于`radix`参数所指定的基数中的字符那么该字符和其后的字符都将被忽略。接着返回已经解析的整数部分。`parseInt`将截取整数部分。开头和结尾的空白符允许存在，会被忽略。
在没有指定基数，或者基数为0的情况下，JavaScript作如下处理：
1. 如果字符串`string`以"0x"或者"0X"开头,则基数是16(16进制).
2. 如果字符串`string`以"0"开头,基数是8（八进制）或者10（十进制），那么具体是哪个基数由实现环境决定。ECMAScript5规定使用10，但是并不是所有的浏览器都遵循这个规定。因此，永远都要明确给出radix参数的值。
3. 如果字符串`string`以其它任何值开头，则基数是10(十进制)。
4. 如果第一个字符不能被转换成数字，`parseInt`返回`NaN`。

算术上，`NaN`不是任何一个进制下的数。你可以调用`isNaN`来判断`parseInt`是否返回`NaN`。`NaN`参与的数学运算其结果总是`NaN`。
将整型数值以特定基数转换成它的字符串值可以使用`intValue.toString(radix)`.

## null与undefined
值`null`是一个 JavaScript 字面量，表示空值（null or an "empty" value），即没有对象被呈现（no object value is present）。它是JavaScript原始数据类型之一。

全局属性`undefined`表示原始值`undefined`。它是一个JavaScript的原始数据类型 。
JavaScript的原始数据类型：`String`,`Number`,`Bollean`,`null`,`undefined`,`symbol`(ES2015新增)

`null`与`undefined`的不同点：
```
typeof null        // object (因为一些以前的原因而不是'null')
typeof undefined   // undefined
null === undefined // false
null  == undefined // true
null === null // true
null == null // true
!null //true
isNaN(1 + null) // false
isNaN(1 + undefined) // true
```

## js精度丢失
计算机的二进制实现和位数限制有些数无法有限表示。就像一些无理数不能有限表示，如 圆周率 3.1415926...，1.3333... 等。JS 遵循 IEEE 754 规范，采用双精度存储（double precision），占用 64 bit。
解决方案：
- 对于整数，前端出现问题的几率可能比较低，毕竟很少有业务需要需要用到超大整数，只要运算结果不超过 Math.pow(2, 53) 就不会丢失精度。
- 对于小数，前端出现问题的几率还是很多的，尤其在一些电商网站涉及到金额等数据。解决方式：把小数放到位整数（乘倍数），再缩小回原来倍数（除倍数）

## switch
`switch()`使用`===`判断相等
______________
2017.08.19更新
______________
## Number.prototype.toString()使用注意 
`Number`对象使用`toString`方法，`Number`对象必须加上`()`,否则表示小数。不加括号会报错。

______________
2017.08.20更新
______________
## RegExp.prototype.exec()

## RegExp.prototype.test()

## Date对象
### 构造函数
```
new Date();//访问当前时间
new Date(value);
new Date(dateString);
new Date(year, month[, day[, hour[, minutes[, seconds[, milliseconds]]]]]);
```
### 参数
`value`
代表自1970年1月1日00:00:00 (世界标准时间) 起经过的毫秒数。
`dateString``
表示日期的字符串值。该字符串应该能被`Date.parse()`方法识别(符合[IETF-compliant RFC 2822 timestamps](https://tools.ietf.org/html/rfc2822#page-14)或[version of ISO8601](http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15))。
`year`
代表年份的整数值。为了避免2000年问题最好指定4位数的年份; 使用 1998, 而不要用 98.
`month`
**代表月份的整数值从0（1月）到11（12月）。**
`day`
代表一个月中的第几天的整数值，从1开始。
`hour`
代表一天中的小时数的整数值 (24小时制)。
`minute`
分钟数。
`second`
秒数。
`millisecond`
表示时间的毫秒部分的整数值。

### 注意
>Note 1: 需要注意的是只能通过调用`Date`构造函数来实例化日期对象：以常规函数调用它（即不加`new`操作符）将会返回>一个字符串，而不是一个日期对象。另外，不像其他JavaScript 类型，`Date`对象没有字面量格式。

>Note2 : 当`Date`作为构造函数调用并传入多个参数时，如果数值大于合理范围时（如月份为13或者分钟数为70），相邻的数值会被调整。比如`new Date(2013, 13, 1)`等于`new Date(2014, 1, 1)`，它们都表示日期2014-02-01（注意月份是从0开始的）。其他数值也是类似，`new Date(2013, 2, 1, 0, 70)`等于`new Date(2013, 2, 1, 1, 10)`，都表示时间2013-03-01T01:10:00。

## Date.parse()
`Date.parse()`方法解析一个表示某个日期的字符串，并返回从1970-1-1 00:00:00 UTC 到该日期对象（该日期对象的UTC时间）的毫秒数，如果该字符串无法识别，或者一些情况下，包含了不合法的日期数值（如：2015-02-31），则返回值为`NaN`。

### 语法
显示调用：
```
Date.parse(dateString)
```
隐式调用：
```
new Date(dateString)
```
### 参数
`dateString`
一个符合[IETF-compliant RFC 2822 timestamps](https://tools.ietf.org/html/rfc2822#page-14)或[version of ISO8601](http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15)日期格式的字符串（其他格式也许也支持，但结果可能与预期不符）。
### 返回值
一个表示从1970-1-1 00:00:00 UTC到给定日期字符串所表示时间的毫秒数的数值。如果参数不能解析为一个有效的日期，则返回NaN。
描述

## Date.prototype.getDay()
### 语法
```
dateObj.getDay()
```
### 参数
无
### 返回值
**`getDay()`返回一个整数值：0代表星期日,1代表星期一,2代表星期二,依次类推。**

## Date.prototype.getDate()
### 语法
```
dateObj.getDate()
```
### 参数
无
### 返回值
**`getDate()`返回一个1到31的整数值。**

## 
## 语法
```
dateObj.getMonth()
```
### 参数
无
### 返回值
**`getMonth`返回一个0到11的整数值：0代表一月份，1代表二月分，2 代表三月份，依次类推。**

## 正则表达式

## RegExp

`g`
全局匹配;找到所有匹配，而不是在第一个匹配后停止


## 参考文献
1. [MDN-Date](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date)
2. [MDN-Date.parse()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/parse)
3. [MDN-Date.prototype.getDay()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/getDay)
4. [MDN-Date.prototype.getDate()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/getDate)
5. [MDN-Date.prototype.getMonth()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/getMonth)
6. [MDN-正则表达式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions)
7. [MDN-RegExp](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp)