---
title: Javascript函数和作用域总结
date: 2017-08-05 22:53:00
tags: [js]
categories: 
    - Javascript
---

## 一、执行环境和作用域
执行环境(execution context)定义了函数或变量有权访问的其他数据，决定了它们各自的行为。每个执行环境都有一个与之关联的变量对象(variable object)，环境中定义的所有变量和函数都保存在这个对象中。全局执行环境是最外围的一个执行环境。某个执行环境的所有代码执行完毕后，该环境销毁，保存在其中的所有变量和函数定义也随之销毁。（全局执行环境直到应用程序退出才会销毁）每个函数都有自己的执行环境。当执行流程进入一个函数时，函数的环境就会被推入一个环境栈中，在函数执行之后，栈将其环境弹出，把控制权返回给之前的执行环境。

当代码在一个环境中执行时，会创建变量对象的一个作用域链(scope chain)。作用域的用途是保证对执行环境有权访问的所有变量和函数的有序访问。作用域链的前端始终是当前执行的代码所在的环境的变量对象。如果这个环境是函数，则将其活动对象(activation object)作为变量对象。活动对象最开始只包含一个变量，即`arguments`对象(这个对象在全局环境中不存在的)，作用域链中的下一个对象来自包含(外部)环境，一直延续到全局执行环境。全局执行环境的变量对象始终都是作用域链的最后一个对象。

标识符解析是沿着作用域链一级一级地搜索标识符的过程。搜索过程中始终从作用域链的前端开始。搜索过程始终从作用域链的前端开始然后逐级地向后回溯，知道找到标识符为止（若找不到，通常会导致错误发生）

js没有块级作用域，但是在es6中新增let关键字可定义块级变量。
## 二、函数
### 定义
1. 函数声明
    ```
    function sum(num1,num2){
        return num1 + num2;
    }
    ```
2. 函数表达式
    ```
    var sum = function(num1,num2){
        return num1 + num2;
    }
    ```
    解析器会率先读取函数声明，并使其在执行任何代码之前可用(可以访问)；
    函数表达式则必须等到解析器执行到它所在的代码行，才会真正被解析执行。
    在代码开始之前，解析器就已通过一个名为函数声明提升(function declaration hoisting)的过程，将函数声明添加到执行环境中。对代码求值时，Javascript引擎在第一遍会声明函数并将他们放到源代码树的顶部。所以，即使声明函数的代码在调用它的代码后面，Javascript引擎也能把函数声明提升到顶部。
3. Function构造函数
    ```
    //接收任意数量参数，最后一个参数始终被看做函数体
    var sum = new Function('num1','num2','return num1 + num2');//不推荐
    ```
    函数是对象，函数名是指针

### 没有重载
同名函数后一个函数会覆盖前一个函数，不会发生函数重载
<!--more-->
## 三、参数传递
1. 基本类型按值传递
```
    function setNum(num){
        num += 10;
        return num;
    }
    var a = 10;
    var result = setNum(a);
    console.log(result);//20
```
2. 引用类型按指针的值传递，并非按引用传递
```
    function setObj(obj){
        obj.name = 'zxlg';
        return obj;
    }
    var person = new Object();
    setObj(person);
    console.log(person.name);// 'zxlg'

//使用对象，看似按引用传递，
//其实不然，参数传递的是指针的值，
//指针的值是不会变的，指针指向的内容有可能会被改变
    function setObj(obj){
        obj.name = 'zxlg';
        obj = new Object();
        obj.name = 'fool';
        return obj;
    }
    var person = new Object();
    setObj(person);
    console.log(person.name);//'zxlg'
```

## 四、arguments对象和this对象
### arguments对象
类数组对象，包含着传入函数中的所有参数，可以用方括号访问它的每一个元素，使用`length`来确定传进多少个元素。函数命名的参数只提供便利，但不是必需的。
```
function doAdd(num1,num2){
    arguments[1] = 10;
    console.log(arguments[0] + num2);
}
```
`arguments`的值永远与对应命名参数的值保持同步。在非严格模式下，重写`arguments[1]`，也就修改了`num2`,结果均变为10(严格模式下重写`arguments`的值会导致语法错误)。但它们的内存空间是独立的，而`arguments`的值会与参数的值同步。

若只传入一个参数，那么`arguments[1]`设置的值不会反应到命名参数中，**因为`arguments`对象的长度是由传入参数的个数决定的，不是由定义函数的参数的个数决定的。**

没有传递值的命名参数自动被赋予`undefined`值，类似于定义变量但没有初始化。

`arguments`的主要用途是保存函数参数，但这个对象还有一个名叫`callee`的属性，该属性是一个指针，指向这个拥有`arguments`对象的函数。
```
//阶乘函数
function factorial(num){
    if(num < 1){
        return 1;
    }else{
        return num * factorial(num - 1);
    }
}
```
函数有名字，函数的执行与函数名factorial紧紧耦合在一起，使用`arguments.callee`可消除这种耦合。
```
function factorial(num){
    if(num < 1){
        return 1;
    }else{
        return num * arguments.callee(num - 1);
    }
}
```

### this对象
this引用的是函数执行的环境对象，**即调用函数的那个对象**。
参考[阮老师的文章](http://www.ruanyifeng.com/blog/2010/04/using_this_keyword_in_javascript.html)
- **纯粹的函数调用**
    这是函数的最通常用法，属于全局性调用，因此this就代表全局对象Global。
    ```
    var x = 1;
　　function test(){
　　　　this.x = 0;
　　}
　　test();
　　alert(x); //0 ```

- **作为对象方法的调用**
    函数还可以作为某个对象的方法调用，这时this就指这个上级对象。
    ```
    function test(){
　　　　alert(this.x);
　　}
　　var o = {};
　　o.x = 1;
　　o.m = test;
　　o.m(); // 1 ```

- **作为构造函数调用**
    所谓构造函数，就是通过这个函数生成一个新对象（object）。这时，this就指这个新对象。
    ```
    var x = 2;
　　function test(){
　　　　this.x = 1;
　　}
　　var o = new test();
　　alert(x); //2```

- **apply调用**
    apply()是函数对象的一个方法，它的作用是改变函数的调用对象，它的第一个参数就表示改变后的调用这个函数的对象。因此，this指的就是这第一个参数。 
    ```
    var x = 0;
　　function test(){
　　　　alert(this.x);
　　}
　　var o={};
　　o.x = 1;
　　o.m = test;
　　o.m.apply(); //0
　　o.m.apply(o); //1 ```

## 五、prototype属性

## 六、变量提升
变量提升(Hoisting)被认为是思考执行上下文（特别是创建和执行阶段）在JavaScript中如何工作的一般方式。但变量提升(Hoisting)可能会导致误解。例如，提升教导变量和函数声明被物理移动到编码的顶部，这不算什么。**真正发生的什么是在编译阶段将变量和函数声明放入内存中，但仍然保留在编码中键入的位置。**
```
/**
* 不推荐的方式：先调用函数，再声明函数 
*/

catName("Chloe");

function catName(name) {
    console.log("My cat's name is " + name);
}

/*
The result of the code above is: "My cat's name is Chloe"
*/

// 等价于

/*函数声明提升*/
function catName(name) {
    console.log("My cat's name is " + name);
}

catName("Tigger");

/*
The result of the code above is: "My cat's name is Chloe"
*/
```
即使我们先在代码中调用函数，在写该函数之前，代码仍然可以工作。

Hoisting 也适用于其他数据类型和变量。变量可以在声明之前进行初始化和使用。但是如果没有初始化，就不能使用。
**JavaScript 仅提升声明，而不是初始化。**如果你使用的是在使用后声明和初始化的一个变量，那么该值将是 undefined。以下两个示例演示了相同的行为。
```
var x = 1; // 声明 + 初始化 x

console.log(x + " " + y);  // y 是未定义的

var y = 2;// 声明 + 初始化 y


//上面的代码和下面的代码是一样的 

var x = 1; // 声明 + 初始化 x

var y; //声明 y

console.log(x + " " + y);  //y 是未定义的

y = 2; // 初始化  y
```
**ES6 : let 不存在 Hoisting**
## 七、检测类型 
1. 检测基本数据类型：`typeof`
```
var str = 'zxlg'; console.log(typeof str); // string
var num = 9; console.log(typeof num);// number
var bool = true; console.log(typeof bool);// boolean
var u; console.log(typeof u);// undefined
var nul = null; console.log(typeof nul);// object
var obj = new Object(); console.log(typeof obj);// object
```

2. 检测引用类型：`instanceof`
`typeof`检测所有引用类型均为`object`，想得到何种引用类型可使用`instanceof`
-----------------
(2017.8.7新增)
检测一个引用类型值和`Object`构造函数时，`instanceof`操作符始终会返回`true`。
**如果使用`instanceof`操作符检测基本类型的值，则该操作符始终会返回`false`,因为基本类型都不是对象。**

```
var arr = [1,2,3];
console.log(arr instanceof Object); // true
console.log(arr instanceof Array); // true
var reg = /\d+/;
console.log(arr instanceof Array); // false
console.log(arr instanceof RegExp); // true

```

## 参考文献
1. [MDN变量提升](https://developer.mozilla.org/zh-CN/docs/Glossary/Hoisting)
2. Javascript高级程序设计(第3版)
3. [Javascript的this用法](http://www.ruanyifeng.com/blog/2010/04/using_this_keyword_in_javascript.html)
