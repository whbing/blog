---
title: JavaScript Puzzlers解析
date: 2017-08-12 21:22:18
tags:
	- js
categories:
	- Javascript
---


[JavaScript Puzzlers!](http://javascript-puzzlers.herokuapp.com/)，题目基于ECMA 262 (5.1)的浏览器环境
______________
2017.08.15更新
______________
### 1.What is the result of this expression? (or multiple ones)   
```      
["1", "2", "3"].map(parseInt)
        
A.["1", "2", "3"]
B.[1, 2, 3]
C.[0, 1, 2]
D.other
```
考察`map`和`parseInt`
`Array.prototype.map()`接收三个参数`(element,index,Array)`
`parseInt()`接收两个参数`(val,radix)`,`radix`为基数，`parseInt('17',8) //15`,`radix`为0或无表示以10为基数。每个位上的数字不能比基数大，否则返回`NaN`,`radix`不能为1,范围为2-36。
```
parseInt('1',0)//1
parseInt('2',1)//每个位上的数字不能比基数大，且基数不能为1,返回NaN
parseInt('3',2)//每个位上的数字不能比基数大,返回NaN
```
所以选D

### 2.What is the result of this expression? (or multiple ones)
```          
[typeof null, null instanceof Object]
        
A.["object", false]
B.[null, false]
C.["object", true]
D.other
```
考察`typeof`和`instanceof`
`typeof`检测非基本对象时，均返回`object`
`null`为基本类型，所以`null instanceof Object // false`
选A

### 3.What is the result of this expression? (or multiple ones)

```  
[ [3,2,1].reduce(Math.pow), [].reduce(Math.pow) ]
        
A.an error
B.[9, 0]
C.[9, NaN]
D.[9, undefined]
```
考察`reduce`函数的用法
`Array.prototype.reduce()`接收两个参数`(function(accumulator, currentValue, currentIndex, array), initialValue)`空数组调用`reduce`时没有设置初始值将会报错。
所以选A

### 4.What is the result of this expression? (or multiple ones)
```         
var val = 'smtg';
console.log('Value is ' + (val === 'smtg') ? 'Something' : 'Nothing');
        
A.Value is Something
B.Value is Nothing
C.NaN
D.other
```
考察运算符优先级，[MDN运算符优先级表](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)
+(13级)优先级大于？(3级),所以`'Value is ' + (val === 'smtg')`条件判断为`true`，答案为`'Something'`,选D

### 5.What is the result of this expression? (or multiple ones)
```          
var name = 'World!';
(function () {
    if (typeof name === 'undefined') {
        var name = 'Jack';
        console.log('Goodbye ' + name);
    } else {
        console.log('Hello ' + name);
    }
})();
        
A.Goodbye Jack
B.Hello Jack
C.Hello undefined
D.Hello World
```
考察[变量声明提升](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/var)（var hoisting）
由于变量声明（以及其他声明）总是在任意代码执行之前处理的，所以在代码中的任意位置声明变量总是等效于在代码开头声明。这意味着变量可以在声明之前使用，这个行为叫做“hoisting”。“hoisting”就像是把所有的变量声明移动到函数或者全局代码的开头位置。
所以选A


### 6.What is the result of this expression? (or multiple ones)
```          
var END = Math.pow(2, 53);
var START = END - 100;
var count = 0;
for (var i = START; i <= END; i++) {
    count++;
}
console.log(count);
        
A.0
B.100
C.101
D.other
```
考察JavaScript中的安全整数范围
`Number.MIN_SAFE_INTEGER`代表在JavaScript中最小的安全的`integer`型数字`-(2^53 - 1)`.
`Number.MAX_SAFE_INTEGER`代表在JavaScript中最大的安全整数`(2^53 - 1)`。
这个数字形成的原因是，Javascript 使用 IEEE 754 中规定的`double-precision floating-point format numbers`，在这个规定中能安全的表示数字的范围在`-(2^53 - 1)`到`2^53 - 1`之间，包含`-(2^53 - 1)`和`2^53 - 1`。
2^53+1 与 2^53超过安全整数范围，`2^53+1 == 2^53//true`,所以为无限循环，选D

### 7.What is the result of this expression? (or multiple ones)
```
var ary = [0,1,2];
ary[10] = 10;
ary.filter(function(x) { return x === undefined;});
        
A.[undefined × 7]
B.[0, 1, 2, 10]
C.[]
D.[undefined]
```
考察`filter`函数
`filter`函数中`callback`只会在已经赋值的索引上被调用，对于那些已经被删除或者从未被赋值的索引不会被调用。
所以选C

### 8.What is the result of this expression? (or multiple ones)
```     
var two   = 0.2
var one   = 0.1
var eight = 0.8
var six   = 0.6
[two - one == one, eight - six == two]
        
A.[true, true]
B.[false, false]
C.[true, false]
D.other
```
考察js大数和浮点数精度丢失的问题
计算机的二进制实现和位数限制有些数无法有限表示。就像一些无理数不能有限表示，如 圆周率 3.1415926...，1.3333... 等。JS 遵循 IEEE 754 规范，采用双精度存储（double precision），占用 64 bit。
解决方案：
对于整数，前端出现问题的几率可能比较低，毕竟很少有业务需要需要用到超大整数，只要运算结果不超过 Math.pow(2, 53) 就不会丢失精度。
对于小数，前端出现问题的几率还是很多的，尤其在一些电商网站涉及到金额等数据。解决方式：把小数放到位整数（乘倍数），再缩小回原来倍数（除倍数）
选择C，没有道理，时而准确时而不准，忧伤


### 9.What is the result of this expression? (or multiple ones)
```          
function showCase(value) {
    switch(value) {
    case 'A':
        console.log('Case A');
        break;
    case 'B':
        console.log('Case B');
        break;
    case undefined:
        console.log('undefined');
        break;
    default:
        console.log('Do not know!');
    }
}
showCase(new String('A'));
        
A.Case A
B.Case B
C.Do not know!
D.undefined
```
考察`switch()`和判等
`switch()`使用`===`判等，而`new String('A')!=='A'`,所以选C

### 10.What is the result of this expression? (or multiple ones)
```
function showCase2(value) {
    switch(value) {
    case 'A':
        console.log('Case A');
        break;
    case 'B':
        console.log('Case B');
        break;
    case undefined:
        console.log('undefined');
        break;
    default:
        console.log('Do not know!');
    }
}
showCase2(String('A'));
        
A.Case A
B.Case B
C.Do not know!
D.undefined
```
考察`String()`方法和判等
`String('A')`没有新建一个对象，而是返回一个`String`，所以选A

______________
2017.08.16更新
______________
### 11.What is the result of this expression? (or multiple ones)
```
function isOdd(num) {
    return num % 2 == 1;
}
function isEven(num) {
    return num % 2 == 0;
}
function isSane(num) {
    return isEven(num) || isOdd(num);
}
var values = [7, 4, '13', -9, Infinity];
values.map(isSane);

A.[true, true, true, true, true]
B.[true, true, true, true, false]
C.[true, true, true, false, false]
D.[true, true, false, false, false]
```
考察取余操作符
取余操作符保证符号，所以`-9 % 2 == -1 ，Infinity % 2 == NaN`
所以选C

### 12.What is the result of this expression? (or multiple ones)
``` 
parseInt(3, 8)
parseInt(3, 2)
parseInt(3, 0)
        
A.3, 3, 3
B.3, 3, NaN
C.3, NaN, NaN
D.other
```
考察`parseInt()`函数，分析见第1题，答案应该为`3,NaN,3` 所以选D

### 13.What is the result of this expression? (or multiple ones)
```
Array.isArray( Array.prototype )
        
A.true
B.false
C.error
D.other
```
考察`Array.prototype`，是数组，所以选A

### 14.What is the result of this expression? (or multiple ones)
```
var a = [0];
if ([0]) {
  console.log(a == true);
} else {
  console.log("wut");
}
        
A.true
B.false
C."wut"
D.other
```
考察`if()`和判等,见[js相等性比较](http://happylg.cn/2017/08/04/js-equal/)
`if([])`都会执行，所以`if([0])`更会执行，对象和布尔值比较，布尔值转换为数字，对象转换为原始值比较,`[0] == true // false`
所以选B

### 15.What is the result of this expression? (or multiple ones)
``` 
[]==[]
        
A.true
B.false
C.error
D.other
```
考察相等性比较，见[js相等性比较](http://happylg.cn/2017/08/04/js-equal/)
对象和对象比较，引用相同返回`true`,否则返回`false`,所以选B

### 16.What is the result of this expression? (or multiple ones)
```   
'5' + 3
'5' - 3
   
A."53", 2
B.8, 2
C.error
D.other
```
考察`+`,`-`运算符,字符串会使用`+`运算符做连接，但是遇到`-`则转换为数值进行运算
所以选A 

### 17.What is the result of this expression? (or multiple ones)
```    
1 + - + + + - + 1
        
A.2
B.1
C.error
D.other
```
考察一元`+, -`
中间一元加号不影响，两个一元减号负负得正变加号,所以选A

### 18.What is the result of this expression? (or multiple ones)
```
var ary = Array(3);
ary[0]=2
ary.map(function(elem) { return '1'; });
        
A.[2, 1, 1]
B.["1", "1", "1"]
C.[2, "1", "1"]
D.other
```
考察`map`函数
删除或未赋值的元素不会被遍历到，所以选D

### 19.What is the result of this expression? (or multiple ones)
```      
function sidEffecting(ary) {
  ary[0] = ary[2];
}
function bar(a,b,c) {
  c = 10
  sidEffecting(arguments);
  return a + b + c;
}
bar(1,1,1)
        
A.3
B.12
C.error
D.other
```
考察函数的`arguments`
函数的`arguments`与参数是相对应的，但不是同一片内存空间，所以答案为21，选D

### 20.What is the result of this expression? (or multiple ones)
```  
var a = 111111111111111110000,
    b = 1111;
a + b;
        
A.111111111111111111111
B.111111111111111110000
C.NaN
D.Infinity
```
考察js的大数精度
js中的大数精度也缺失了，选B 

### 21.What is the result of this expression? (or multiple ones)
```
var x = [].reverse;
x();

A.[]
B.undefined
C.error
D.window
```
题目基于ECMA 262 (5.1)的浏览器环境，`[].reverse`返回`this`，被浏览器调用之后为`window`。
选D

### 22.What is the result of this expression? (or multiple ones)
```   
Number.MIN_VALUE > 0
        
A.false
B.true
C.error
D.other
```
考察`Number`
`Number.MIN_VALUE`是大于0的最小数，所以选B

### 23.What is the result of this expression? (or multiple ones)
```
[1 < 2 < 3, 3 < 2 < 1]
        
A.[true, true]
B.[true, false]
C.error
D.other
```
考察`<`运算符
`(1 < 2) < 3`,`1 < 2 //true 转换为1` `1 < 3 // true`
`(3 < 2) < 1`,`3 < 2 //false 转换为0` `0 < 1 //true`
所以选A

### 24.What is the result of this expression? (or multiple ones)
```   
// the most classic wtf
2 == [[[2]]]
        
A.true
B.false
C.undefined
D.other
```
考察`==`
对象和其他值比较时，会将对象转换为原始值，`[[[2]]]`转换原始值为2
所以返回`true`

### 25.What is the result of this expression? (or multiple ones)
```
3.toString()
3..toString()
3...toString()
        
A."3", error, error
B."3", "3.0", error
C.error, "3", error
D.other
```


### 26.What is the result of this expression? (or multiple ones)

          
(function(){
  var x = y = 1;
})();
console.log(y);
console.log(x);
        
1, 1
error, error
1, error
other

What is the result of this expression? (or multiple ones)

          
var a = /123/,
    b = /123/;
a == b
a === b
        
true, true
true, false
false, false
other

What is the result of this expression? (or multiple ones)

          
var a = [1, 2, 3],
    b = [1, 2, 3],
    c = [1, 2, 4]
a ==  b
a === b
a >   c
a <   c
        
false, false, false, true
false, false, false, false
true, true, false, true
other

What is the result of this expression? (or multiple ones)

          
var a = {}, b = Object.prototype;
[a.prototype === b, Object.getPrototypeOf(a) === b]
        
[false, true]
[true, true]
[false, false]
other


What is the result of this expression? (or multiple ones)

          
function f() {}
var a = f.prototype, b = Object.getPrototypeOf(f);
a === b
        
true
false
null
other

What is the result of this expression? (or multiple ones)

          
function foo() { }
var oldName = foo.name;
foo.name = "bar";
[oldName, foo.name]
        
error
["", ""]
["foo", "foo"]
["foo", "bar"]

What is the result of this expression? (or multiple ones)

          
"1 2 3".replace(/\d/g, parseInt)
        
"1 2 3"
"0 1 2"
"NaN 2 3"
"1 NaN 3"

What is the result of this expression? (or multiple ones)

          
function f() {}
var parent = Object.getPrototypeOf(f);
f.name // ?
parent.name // ?
typeof eval(f.name) // ?
typeof eval(parent.name) //  ?
        
"f", "Empty", "function", "function"
"f", undefined, "function", error
"f", "Empty", "function", error
other

What is the result of this expression? (or multiple ones)

          
var lowerCaseOnly =  /^[a-z]+$/;
[lowerCaseOnly.test(null), lowerCaseOnly.test()]
        
[true, false]
error
[true, true]
[false, true]

What is the result of this expression? (or multiple ones)

          
[,,,].join(", ")
        
", , , "
"undefined, undefined, undefined, undefined"
", , "
""

What is the result of this expression? (or multiple ones)

          
var a = {class: "Animal", name: 'Fido'};
a.class
        
"Animal"
Object
an error
other

What is the result of this expression? (or multiple ones)

          
var a = new Date("epoch")
        
Thu Jan 01 1970 01:00:00 GMT+0100 (CET)
current time
error
other

What is the result of this expression? (or multiple ones)

          
var a = Function.length,
    b = new Function().length
a === b
        
true
false
error
other

What is the result of this expression? (or multiple ones)

          
var a = Date(0);
var b = new Date(0);
var c = new Date();
[a === b, b === c, a === c]
        
[true, true, true]
[false, false, false]
[false, true, false]
[true, false, false]

What is the result of this expression? (or multiple ones)

          
var min = Math.min(), max = Math.max()
min < max
        
true
false
error
other

What is the result of this expression? (or multiple ones)

          
function captureOne(re, str) {
  var match = re.exec(str);
  return match && match[1];
}
var numRe  = /num=(\d+)/ig,
    wordRe = /word=(\w+)/i,
    a1 = captureOne(numRe,  "num=1"),
    a2 = captureOne(wordRe, "word=1"),
    a3 = captureOne(numRe,  "NUM=2"),
    a4 = captureOne(wordRe,  "WORD=2");
[a1 === a2, a3 === a4]
        
[true, true]
[false, false]
[true, false]
[false, true]

What is the result of this expression? (or multiple ones)

          
var a = new Date("2014-03-19"),
    b = new Date(2014, 03, 19);
[a.getDay() === b.getDay(), a.getMonth() === b.getMonth()]
        
[true, true]
[true, false]
[false, true]
[false, false]

What is the result of this expression? (or multiple ones)

          
if ('http://giftwrapped.com/picture.jpg'.match('.gif')) {
  'a gif file'
} else {
  'not a gif file'
}
        
'a gif file'
'not a gif file'
error
other

What is the result of this expression? (or multiple ones)

          
function foo(a) {
    var a;
    return a;
}
function bar(a) {
    var a = 'bye';
    return a;
}
[foo('hello'), bar('hello')]
        
["hello", "hello"]
["hello", "bye"]
["bye", "bye"]
other