---
title: Javascript原型对象和原型链
date: 2017-08-20 16:22:56
tags: [js,原型,原型对象,prototype]
categories: 
	- Javascript
---
## 函数原型对象和原型
```
function Person(){
}
Person.prototype.name = "zxlg";
Person.prototype.age = 24;
Person.prototype.sayName = function(){
	console.log(this.name);
};
var person1 = new Person();
person1.sayName(); //"zxlg"
var person2 = new Person();
person2.sayName(); //"zxlg"
person1.sayName == person2.sayName; //true
```

无论什么时候，只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个`prototype`属性，这个属性指向函数的原型对象。在默认情况下，所有原型对象都会自动获得一个`constructor`（构造函数）属性，这个属性包含一个指向 `prototype`属性所在函数的指针。上述代码中`Person.prototype.constructor`指向`Person`。

创建了自定义的构造函数之后，其原型对象默认只会取得`constructor`属性；至于其他方法，则都是从`Object`继承而来的。当调用构造函数创建一个新实例后，该实例的内部将包含一个指针（内部属性），指向构造函数的原型对象。 ECMA-262 第 5 版中管这个指针叫`[[Prototype]]`。虽然在脚本中没有标准的方式访问`[[Prototype]]`，但 Firefox、 Safari 和 Chrome 在每个对象上都支持一个属性`__proto__`；而在其他实现中，这个属性对脚本则是完全不可见的。
**这个连接存在于实例与构造函数的原型对象之间，而不是存在于实例与构造函数之间。**
上述代码中`person1`和`person2`的`[[Prototype]]`指向构造函数的原型对象`Person.prototype`

### 获取[[Prototype]]
可以通过`isPrototypeOf()`方法来确定对象之间是否存在这种关系。从本质上讲，如果`[[Prototype]]`指向调用 `isPrototypeOf()`方法的对象（`Person.prototype`），那么这个方法就返回`true`。
```
Person.prototype.isPrototypeOf(person1); //true
Person.prototype.isPrototypeOf(person2); //true
```
可以通过`Object.getPrototypeOf()`方法返回`[[Prototype]]`的值。
Object.getPrototypeOf(person1) == Person.prototype; //true
Object.getPrototypeOf(person1).name; //"zxlg"

### 原型链
每当代码读取某个对象的某个属性时，都会执行一次搜索，目标是具有给定名字的属性。搜索首先从对象实例本身开始。如果在实例中找到了具有给定名字的属性，则返回该属性的值；如果没有找到，则继续搜索指针指向的原型对象，在原型对象中查找具有给定名字的属性。如果在原型对象中找到了这个属性，则返回该属性的值。直到最顶端，也就是原型链的实现过程。

### 重写原型的值
虽然可以通过对象实例访问保存在原型中的值，但却不能通过对象实例重写原型中的值。如果我们在实例中添加了一个属性，而该属性与实例原型中的一个属性同名，那我们就在实例中创建该属性，该属性将会屏蔽原型中的那个属性。添加这个属性只会阻止我们访问原型中的那个属性，但不会修改那个属性。即使将这个属性设置为`null`，也只会在实例中设置这个属性，而不会恢复其指向原型的连接。但使用`delete`操作符则可以完全删除实例属性，从而让我们能够重新访问原型中的属性。
```
person1.name = "Tom"
person1.sayName(); //"Tom"
delete person1.name;
person1.sayName(); //"zxlg"
```
### 属性访问
1. `in`
	`in`操作符会在通过对象能够访问给定属性时返回`true`，无论该属性存在于实例中还是原型中。
2. `hasOwnProperty()`
	使用`hasOwnProperty()`方法可以检测一个属性是存在于实例中，还是存在于原型中。这个方法 （不要忘了它是从 `Object`继承来的）只在给定属性存在于对象实例中时，才会返回`true`。
3. `Object.keys()`
	要取得对象上所有可枚举的实例属性，可以使用 ECMAScript 5 的`Object.keys()`方法

### 原型语法
前面例子中每添加一个属性和方法就要敲一遍`Person.prototype`。为减少不必要的输入，也为了从视觉上更好地封装原型的功能，更常见的做法是用一个包含所有属性和方法的对象字面量来重写整个原型对象
```
function Person(){
}
Person.prototype = {
	name : "zxlg",
	age : 24,
	sayName : function () {
	console.log(this.name);
	}
};
```
在上面的代码中，将`Person.prototype`设置为等于一个以对象字面量形式创建的新对象。最终结果相同，但有一个例外： `constructor`属性不再指向`Person`。**每创建一个函数，就会同时创建它的`prototype`对象，这个对象也会自动获得 `constructor`属性。而我们在这里使用的语法，本质上完全重写了默认的`prototype`对象，因此`constructor`属性也就变成了新对象的`constructor`属性 （指向`Object`构造函数），不再指向`Person`函数。**
此时尽管`instanceof`操作符还能返回正确的结果，但通过`constructor`已经无法确定对象的类型了，如下所示。
```
var friend = new Person();
friend instanceof Object; //true
friend instanceof Person; //true
friend.constructor == Person; //false
friend.constructor == Object; //true
```
如果`constructor`的值真的很重要，可以将其加入`prototype`中,并使用`Object.defineProperty()`将其`[[Enumerable]]`特性被设置为`false`

### 原型的动态性
由于在原型中查找值的过程是一次搜索，因此我们对原型对象所做的任何修改都能够立即从实例上反映出来——即使是先创建了实例后修改原型也照样如此。
```
var friend = new Person();
Person.prototype.sayHi = function(){
	alert("hi");
};
friend.sayHi(); //"hi"（没有问题！）
```
尽管可以随时为原型添加属性和方法，并且修改能够立即在所有对象实例中反映出来，但如果是重写整个原型对象，那么情况就不一样了。调用构造函数时会为实例添加一个指向最初原型的`[[Prototype]]`指针，而把原型修改为另外一个对象就等于切断了构造函数与最初原型之间的联系。
**实例中的指针仅指向原型，而不指向构造函数。**
```
function Person() {
}
var friend = new Person();
Person.prototype = {
  constructor: Person,
  name: "zxlg",
  age: 24,
  sayName: function () {
    alert(this.name);
  }
};
friend.sayName(); //error
```
重写原型对象切断了现有原型与任何之前已经存在的对象实例之间的联系，它们引用的仍然是最初的原型

## 原生对象的原型
所有原生的引用类型都是采用这种模式创建的。所有原生引用类型（`Object`、` Array`、 `String`，等等）都在其构造函数的原型上定义了方法。

## 参考文献
1. [一个例子让你彻底明白原型对象和原型链](http://www.jianshu.com/p/aa1ebfdad661)
2. Javascript高级程序编程(第三版)