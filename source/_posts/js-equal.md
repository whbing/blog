---
title: Javascript相等性比较
date: 2017-08-04 09:43:39
tags:
	- js
categories:
	- Javascript
---


JavaScript有两种比较方式：严格比较运算符和转换类型比较运算符。对于严格比较运算符（`===`）来说，仅当两个操作数的类型相同且值相等为`true`，而对于被广泛使用的比较运算符（`==`）来说，会在进行比较之前，将两个操作数转换成相同的类型。
对于关系运算符（比如 `<=`）来说，会先将操作数转为原始值，使它们类型相同，再进行比较运算。字符串比较则是使用基于标准字典的Unicode值来进行比较的。数组也是这样比较的。

## 一、宽松相等(==)
比较操作符会为两个不同类型的操作数转换类型，然后进行严格比较。当两个操作数都是对象时，JavaScript会比较其内部引用，当且仅当他们的引用指向内存中的相同对象（区域）时才相等，即他们在栈内存中的引用地址相同。

- 当比较数字和字符串时，字符串会转换成数字值。 JavaScript 尝试将数字字面量转换为数字类型的值。 首先, 一个数学上的值会从数字字面量中衍生出来，然后得到被四舍五入后的数字类型的值。
- 如果其中一个操作数为布尔类型，那么布尔操作数如果为true，那么会转换为1，如果为false，会转换为整数0，即0。
- 如果是一个对象与数字或字符串向比较，JavaScript会尝试返回对象的默认值。操作符会尝试将对象转换为其原始值（一个字符串或数字值）通过方法valueOf和toString。如果尝试转换失败，会产生一个运行时错误。
- 注意：当且仅当与原始值比较时，对象会被转换为原始值。**当两个操作数均为对象时，它们作为对象进行比较，仅当它们引用相同对象时返回true**。

<img src="/img/非严格相等.png"  alt="非严格相等比较" align=center />
在上面的表格中，`ToNumber(A)` 尝试在比较前将参数`A`转换为数字，这与`+A`（单目运算符+）的效果相同。通过尝试依次调用`A`的`A.toString`和`A.valueOf`方法，将参数`A`转换为原始值（`Primitive`）。
一般而言，根据 ECMAScript 规范，所有的对象都与`undefined`和`null`不相等。但是大部分浏览器允许非常窄的一类对象（即，所有页面中的`document.all`对象），在某些情况下，充当效仿`undefined`的角色。相等操作符就是在这样的一个背景下。因此，`IsFalsy(A)`方法的值为`true`，当且仅当`A`效仿`undefined`。在其他所有情况下，一个对象都不会等于`undefined`或`null`。
<!--more-->

## 二、一致/严格相等(===)
一致运算符不会进行类型转换，仅当操作数严格相等时返回`true`
```
var num = 0;
var obj = new String("0");
var str = "0";
var b = false;

console.log(num === num); // true
console.log(obj === obj); // true
console.log(str === str); // true

console.log(num === obj); // false
console.log(num === str); // false
console.log(obj === str); // false
console.log(null === undefined); // false
console.log(obj === null); // false
console.log(obj === undefined); // false
```
对于除了数值之外的值，全等操作符使用明确的语义进行比较：一个值只与自身全等。对于数值，全等操作符使用略加修改的语义来处理两个特殊情况：第一个情况是，浮点数`0`是不分正负的。区分`+0`和`-0`在解决一些特定的数学问题时是必要的，但是大部分境况下我们并不用关心。全等操作符认为这两个值是全等的。第二个情况是，浮点数包含了`NaN`值，用来表示某些定义不明确的数学问题的解，例如：正无穷加负无穷。全等操作符认为`NaN`与其他任何值都不全等，包括它自己。（等式 (`x !== x`) 成立的唯一情况是 `x` 的值为`NaN`）

## 三、Object.is （ECMAScript 2015/ ES6 新特性）
`Object.is`的行为方式与三等式相同，但是对于`NaN`和`-0`和`+0`进行特殊处理，所以最后两个不相同，而`Object.is（NaN，NaN）`将为`true`。(通常使用双等号或三等于将`NaN`与`NaN`进行比较结果为`false`，因为IEEE 754如是说。)除了`+0`和`-0`,其他时候避免使用`Object.is`,包括`NaN`。

## 四、判等表
x|y|==|===|Object.is|
---|---|---|---|---
undefined|undefined|true|true|true
null|null|true|true|true
true|true|true|true|true
false|false|true|true|true
"foo"|"foo"|true|true|true
{ foo: "bar" }|x|true|true|true
0|0|true|true|true
+0|-0|true|true|false
0|false|true|false|false
""|false|true|false|false
""|0|true|false|false
"0"|0|true|false|false
"17"|17|true|false|false
[1,2]|"1,2"|true|false|false
new String("foo")|"foo"|true|false|false
null|undefined|true|false|false
null|false|false|false|false
undefined|false|false|false|false
{ foo: "bar" }|{ foo: "bar" }|false|false|false
new String("foo")|new String("foo")|false|false|false
0|null|false|false|false
0|NaN|false|false|false
"foo"|NaN|false|false|false
NaN|NaN|false|false|true


```
// true 两个操作数均为String
'foo' === 'foo'

var a = new String('foo');
var b = new String('foo');

// false a,b对象引用不同
a == b 

// false a,b均为对象，但是引用不同
a === b 

// true 操作数类型不同，对象a转换为String类型
a == 'foo'
```
## 五、参见
### `==`比较表
<img src="/img/js比较表.png"  alt="js比较表" align=center />

-----------
### `if()`比较表(2017.8.6新增)
<img src="/img/if比较表.png"  alt="if比较表" align=center />

[Javascript比较表](http://dorey.github.io/JavaScript-Equality-Table/)


## 参考文献
1. [MDN比较运算符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Comparison_Operators)
2. [MDN相等性比较](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Equality_comparisons_and_sameness)
3. [ECMAScript官方文件](http://www.ecma-international.org/ecma-262/5.1/#sec-11.9)
4. [js比较表](http://dorey.github.io/JavaScript-Equality-Table/)
