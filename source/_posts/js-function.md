---
title: Javascript中的Function对象总结
date: 2017-08-12 20:17:06
tags: [js]
categories: 
	- Javascript
---
# 属性
## arguments
`arguments`是一个类似数组的对象, 对应于传递给函数的参数。
`arguments`对象是所有函数中可用的局部变量。你可以使用`arguments`对象在函数中引用函数的参数。此对象包含传递给函数的每个参数的条目，第一个条目的索引从0开始。

### 转换数组
`arguments`对象不是一个`Array`。它类似于数组，但除了长度之外没有任何数组属性。例如，它没有`pop`方法。但是它可以被转换为一个真正的数组：
```
let args = Array.prototype.slice.call(arguments); 

let args = [].slice.call(arguments);
```
你还可以使用`Array.from()`方法或`spread`运算符将`arguments`转换为真正的数组：
```
let args = Array.from(arguments);
let args = [...arguments];
```
### 属性
`arguments.callee`
指向当前执行的函数。
`arguments.length`
指向传递给当前函数的参数数量。
## length
`length`是函数对象的一个属性值，指该函数有多少个必须要传入的参数，那些已定义了默认值的参数不算在内，比如`function（xx = 0）`的`length`是0。与之对比的是，`arguments.length`是函数被调用时实际传参的个数。

`Function`构造器本身也是个`Function`。他的`length`属性值为 1 。该属性 `Writable: false`, `Enumerable: false`,` Configurable: true`。

`Function`原型对象的`length`属性值为 0 。
<!--more-->
# 方法
## Function.prototype.apply()
`apply()`方法调用一个函数, 其具有一个指定的`this`值，以及作为一个数组（或类似数组的对象）提供的参数。
### 语法：
```
func.apply(thisArg, [argsArray])
```
### 参数
`thisArg`
在`func`函数运行时指定的`this`值。需要注意的是，指定的`this`值并不一定是该函数执行时真正的`this`值，如果这个函数处于非严格模式下，则指定为`null`或`undefined`时会自动指向全局对象（浏览器中就是window对象），同时值为原始值（数字，字符串，布尔值）的`this`会指向该原始值的自动包装对象。
`argsArray`
一个数组或者类数组对象，其中的数组元素将作为单独的参数传给`func`函数。如果该参数的值为`null`或`undefined`，则表示不需要传入任何参数。从ECMAScript5开始可以使用类数组对象。浏览器兼容性请参阅本文底部内容。

### 描述
在调用一个存在的函数时，你可以为其指定一个`this`对象。`this`指当前对象，也就是正在调用这个函数的对象。 使用 `apply`， 你可以只写一次这个方法然后在另一个对象中继承它，`而不用在新对象中重复写该方法。

`apply`与`call`非常相似，不同之处在于提供参数的方式。`apply`使用参数数组而不是一组参数列表（原文：a named set of parameters）。`apply`可以使用数组字面量（array literal），如 `fun.apply(this, ['eat', 'bananas'])`，或数组对象， 如`fun.apply(this, new Array('eat', 'bananas'))`。

你也可以使用`arguments`对象作为`argsArray`参数。`arguments`是一个函数的局部变量。它可以被用作被调用对象的所有未指定的参数。这样，你在使用apply函数的时候就不需要知道被调用对象的所有参数。 你可以使用`arguments`来把所有的参数传递给被调用对象。 被调用对象接下来就负责处理这些参数。

## Function.prototype.call()
`call()`方法调用一个函数,其具有一个指定的`this`值和分别地提供的参数(参数的列表)。

注意：该方法的作用和`apply()`方法类似，只有一个区别，就是`call()`方法接受的是若干个参数的列表，而`apply()`方法接受的是一个包含多个参数的数组。
### 语法
```
func.call(thisArg[, arg1[, arg2[, ...]]])
```
### 参数
`thisArg`
在`func`函数运行时指定的`this`值。需要注意的是，指定的`this`值并不一定是该函数执行时真正的`this`值，如果这个函数处于非严格模式下，则指定为`null`和`undefined`的`this`值会自动指向全局对象(浏览器中就是window对象)，同时值为原始值(数字，字符串，布尔值)的this会指向该原始值的自动包装对象。
`arg1, arg2, ...`
指定的参数列表。

### 返回值
返回结果包括指定的this值和参数。

### 描述

可以让`call()`中的对象调用当前对象所拥有的`function`。你可以使用`call()`来实现继承：写一个方法，然后让另外一个新的对象来继承它（而不是在新对象中再写一次这个方法）。