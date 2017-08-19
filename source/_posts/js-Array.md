---
title: Javascript中的Array对象总结
date: 2017-08-03 18:23:35
tags: [js]
categories: 
	- Javascript
---
# 一、属性
## Array.length
`length`属性的值是一个 0 到 2^32-1的整数。

你可以通过减小`length`属性的值来截短一个数组，但不能通过增大`length`属性的值来延长这个数组

## Array.prototype
`Array.prototype`属性表示`Array`构造函数的原型;`Array.prototype`本身也是一个`Array`

# 二、方法

## Array.prototype.concat()
**不改变数组**

### 语法
	var new_array = old_array.concat(value1[, value2[, ...[, valueN]]])

### 参数 
`valueN`需要与原数组合并的数组或非数组值。详见下文。

### 返回值 
新的`Array`实例。

### 描述
`concat`方法将创建一个新的数组，然后将调用它的对象(`this`指向的对象)中的元素以及所有参数中的数组类型的参数中的元素以及非数组类型的参数本身按照顺序放入这个新数组,并返回该数组.

`concat`方法并不修改调用它的对象(`this`指向的对象) 和参数中的各个数组本身的值,而是将他们的每个元素拷贝一份放在组合成的新数组中.原数组中的元素有两种被拷贝的方式:
- 对象引用(非对象直接量):`concat`方法会复制对象引用放到组合的新数组里,原数组和新数组中的对象引用都指向同一个实际的对象,所以,当实际的对象被修改时,两个数组也同时会被修改.
- 字符串和数字(是原始值,而不是包装原始值的`String`和`Number`对象):`concat`方法会复制字符串和数字的值放到新数组里

<!--more-->
## Array.prototype.filter()

### 语法
	var new_array = arr.filter(callback[, thisArg])

### 参数
`callback`用来测试数组的每个元素的函数。调用时使用参数`(element, index, array)`。返回`true`表示保留该元素（通过测试），`false`则不保留。
`thisArg`可选。执行`callback`时的用于`this`的值。

### 返回值
一个新的通过测试的元素的集合的数组

### 描述
`filter`为数组中的每个元素调用一次`callback`函数，并利用所有使得`callback`返回`true`或等价于`true`的值的元素创建一个新数组。`callback`只会在已经赋值的索引上被调用，对于那些已经被删除或者从未被赋值的索引不会被调用。那些没有通过`callback`测试的元素会被跳过，不会被包含在新数组中。
`callback`被调用时传入三个参数：
1. 元素的值
2. 元素的索引
3. 被遍历的数组

如果为`filter`提供一个`thisArg`参数，则它会被作为`callback`被调用时的`this`值。否则，`callback`的`this`值在非严格模式下将是全局对象，严格模式下为`undefined`。

**filter不会改变原数组。**
`filter`遍历的元素范围在第一次调用`callback`之前就已经确定了。在调用`filter`之后被添加到数组中的元素不会被filter遍历到。如果已经存在的元素被改变了，则他们传入`callback`的值是`filter`遍历到它们那一刻的值。**被删除或从来未被赋值的元素不会被遍历到。**

## Array.prototype.find()

## Array.prototype.forEach()

### 语法
```
	array.forEach(callback(currentValue, index, array){
		//do something
	}, this)
	array.forEach(callback[, thisArg])
```

### 参数
`callback`为数组中每个元素执行的函数，该函数接收三个参数：
- `currentValue`(当前值)数组中正在处理的当前元素。
- `index`(索引)数组中正在处理的当前元素的索引。
- `array``forEach()`方法正在操作的数组。

`thisArg`可选参数。当执行回调 函数时用作`this`的值(参考对象)。

### 返回值 
`undefined`
### 描述
`forEach`方法按升序为数组中含有效值的每一项执行一次`callback`函数，那些已删除（使用`delete`方法等情况）或者未初始化的项将被跳过（但不包括那些值为`undefined`的项）（例如在稀疏数组上）。
`callback`函数会被依次传入三个参数：
1. 数组当前项的值
2. 数组当前项的索引
3. 数组对象本身

如果给`forEach`传递了`thisArg`参数，当调用时，它将被传给`callback`函数，作为它的`this`值。否则，将会传入`undefined`作为它的`this`值。`callback`函数最终可观察到`this`值，这取决于函数观察到`this`的常用规则。
`forEach`遍历的范围在第一次调用`callback`前就会确定。调用`forEach`后添加到数组中的项不会被`callback`访问到。如果已经存在的值被改变，则传递给`callback`的值是`forEach`遍历到他们那一刻的值。已删除的项不会被遍历到。如果已访问的元素在迭代时被删除了(例如使用`shift()`) ，之后的元素将被跳过.
`forEach()`为每个数组元素执行`callback`函数；不像`map()`或者`reduce()` ，它总是返回`undefined`值，并且不可链式调用。典型用例是在一个链的最后执行副作用。

## Array.prototype.indexOf()
indexOf()方法返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回-1。

### 语法
```
	arr.indexOf(searchElement)
	arr.indexOf(searchElement[, fromIndex = 0])
```

### 参数
`searchElement`要查找的元素。
`fromIndex`开始查找的位置。如果该索引值大于或等于数组长度，意味着不会在数组里查找，返回-1。如果参数中提供的索引值是一个负值，则将其作为数组末尾的一个抵消，即-1表示从最后一个元素开始查找，-2表示从倒数第二个元素开始查找，以此类推。注意：如果参数中提供的索引值是一个负值，仍然从前向后查询数组。如果抵消后的索引值仍小于0，则整个数组都将会被查询。其默认值为0.

### 返回值
首个被找到的元素在数组中的索引位置;若没有找到则返回-1

## Array.prototype.join()
**不改变数组**

## Array.prototype.keys()

## Array.prototype.values()

## Array.prototype.entries()

## Array.prototype.pop()

## Array.prototype.push()

## Array.prototype.shift()

## Array.prototype.unshift()

## Array.prototype.map()
### 语法
```
	let array = arr.map(function callback(currentValue, index, array) { 
    	// Return element for new_array 
	}[, thisArg])
```

### 参数
`callback`生成新数组元素的函数，使用三个参数：
`currentValue``callback`的第一个参数，数组中正在处理的当前元素。
`index``callback`的第二个参数，数组中正在处理的当前元素的索引。
`array``callback`的第三个参数，`map`方法被调用的数组。
`thisArg`可选的。执行`callback`函数时,使用的`this`值。

### 返回值
一个新数组，每个元素都是回调函数的结果。

### 描述
`map`方法会给原数组中的每个元素都按顺序调用一次`callback`函数。`callback`每次执行后的返回值（包括`undefined`）组合起来形成一个新数组。`callback`函数只会在有值的索引上被调用；那些从来没被赋过值或者使用`delete`删除的索引则不会被调用。
`callback`函数会被自动传入三个参数：数组元素，元素索引，原数组本身。
如果`thisArg`参数有值，则每次`callback`函数被调用的时候，`this`都会指向`thisArg`参数上的这个对象。如果省略了`thisArg`参数,或者赋值为`null`或`undefined`，则`this`指向全局对象。
**map不修改调用它的原数组本身**（当然可以在`callback`执行时改变原数组）。
使用`map`方法处理数组时，数组元素的范围是在`callback`方法第一次调用之前就已经确定了。在`map`方法执行的过程中：原数组中新增加的元素将不会被`callback`访问到；若已经存在的元素被改变或删除了，则它们的传递到`callback`的值是`map`方法遍历到它们的那一时刻的值；**而被删除的元素将不会被访问到。**

## Array.prototype.reduce()
### 语法
	array.reduce(function(accumulator, currentValue, currentIndex, array), initialValue)

### 参数
`callback`执行数组中每个值的函数，包含四个参数
`accumulator`上一次调用回调返回的值，或者是提供的初始值`initialValue`
`currentValue`数组中正在处理的元素
`currentIndex`数据中正在处理的元素索引,如果提供了`initialValue`,从0开始;否则从1开始
`array`调用`reduce`的数组
`initialValue`可选项，其值用于第一次调用`callback`的第一个参数。如果没有设置初始值，则将数组中的第一个元素作为初始值。**空数组调用`reduce`时没有设置初始值将会报错。**

### 返回值
函数累计处理的结果

### 描述
`reduce`为数组中的每一个元素依次执行回调函数，不包括数组中被删除或从未被赋值的元素，接受四个参数：
`accumulator`初始值（或者上一次回调函数的返回值）
`currentValue`当前元素值
`currentIndex`当前索引
`array`调用`reduce`的数组。
回调函数第一次执行时，`accumulator`和`currentValue`的取值有两种情况：调用`reduce`时提供`initialValue`，`accumulator`取值为`initialValue`，`currentValue`取数组中的第一个值；没有提供`initialValue`，`accumulator`取数组中的第一个值，`currentValue`取数组中的第二个值。
**注意**: 
1. 不提供`initialValue`，`reduce`会从索引1的地方开始执行`callback`方法，跳过第一个索引。提供`initialValue`，从索引0开始。
2. 如果数组为空并且没有提供`initialValue`，会抛出`TypeError`。如果数组仅有一个元素（无论位置如何）并且没有提供`initialValue`，或者有提供`initialValue`但是数组为空，那么此唯一值将被返回并且`callback`不会被执行。

## Array.prototype.reduceRight()

## Array.prototype.sort()
**改变数组**

## Array.prototype.reverse()
**改变数组**

## Array.prototype.slice()
**注意:'slice()'为浅拷贝,不改变数组**

### 语法
	arr.slice(begin,end)

### 参数
`begin`可选，从索引处开始提取，默认为0。如果参数为负数-a，从倒数第a个元素开始提取。`slice(-2)`表示从倒数第二提取到倒数第一(包含最后一个元素)。
`end`可选，从索引处结束提取。`slice(begin,end)`包含`begin`但不包含`end`。如果为负数表示在倒数第几个结束提取，slice(-2,-1),不包含最后一个。
`end`省略或大于数组长度，一直提取到原数组末尾

### 返回值
一个含有提取元素的新数组

### 描述
`slice()`不会修改数组，只会浅拷贝，规则如下：
- 对象引用(非对象直接量):`concat`方法会复制对象引用放到组合的新数组里,原数组和新数组中的对象引用都指向同一个实际的对象,所以,当实际的对象被修改时,两个数组也同时会被修改.
- 对于字符串、数字及布尔值来说（不是`String`、`Number`或者`Boolean`对象），`slice`会拷贝这些值到新的数组里。在别的数组里修改这些字符串或数字或是布尔值，将不会影响另一个数组。

如果向两个数组任一中添加了新元素，则另一个不会受到影响。

## Array.prototype.splice()
**改变数组，返回被删除元素组成的数组**

# 参考文献
1. [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)
