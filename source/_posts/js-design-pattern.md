---
title: Javascript中的设计模式总结
date: 2017-08-20 16:59:39
tags: [js,设计模式]
categories: 
	- Javascript
---

————————————————————————
2017-08-21 更新
————————————————————————
## 工厂模式
```
function createPerson(name,age){
	var obj = new Object();
	obj.name = name;
	obj.age = age;
	obj.sayName = function(){
		console.log(obj.name)
	} 
	return obj;
}

var person1 = createPerson('Tom',20);
var person2 = createPerson('zxlg',24);
```
工厂模式解决创建多个相似对象的问题，但是**没有解决对象识别的问题，即怎样知道一个变量的类型**


<!-- more -->
## 构造函数模式
```
function Person(name,age){
	obj.name = name;
	obj.age = age;
	obj.sayName = function(){
		console.log(obj.name)
	} 
}

var person1 = new Person('Tom',20);
var person2 = new Person('zxlg',24);
```
`Person()`与`createPerson()`不同之处：
- 没有显式地创建对象；
- 直接将属性和方法赋给了 this 对象；
- 没有`return`语句

要创建实例，必须使用`new`操作符，经历4个步骤
1. 创建一个新对象
2. 将构造函数的作用域赋给新对象
3. 执行构造函数的代码
4. 返回新对象

```
person1.constructor == Person; //true
person2.constructor == Person; //true
person1 instanceof Object; //true
person1 instanceof Person; //true
person2 instanceof Object; //true
person2 instanceof Person; //true
```
所有实例对象都有相同的`constructor`(构造函数属性)，指向构造函数。对象的`constructor`属性最初是用来标识对象类型的，可以用`instanceof`操作符检测对象类型。**创造自定义的构造函数意味着将来可以将它的实例标识为一种特定的类型**,而这正是构造函数模式胜过工厂模式的地方。

### 构造函数当作函数
```
// 当作构造函数使用
var person = new Person("liuli", 23);
person.sayName(); //"liuli"
// 作为普通函数调用
Person("zhangfan", 26); // 添加到 window
window.sayName(); //"zhangfan"
// 在另一个对象的作用域中调用
var o = new Object();
Person.call(o, "chenming", 24);
o.sayName(); //"chenming"
```
不使用`new`操作符调用`Person()`会出现什么结果：属性和方法都被添加给`window`对象了。也可以使用`call()`（或者 `apply()`）在某个特殊对象的作用域中调用`Person()`函数。

### 构造函数缺点
使用构造函数的主要问题，就是每个方法都要在每个实例上重新创建一遍。在前面的例子中，`person1`和`person2`都有一个名为`sayName()`的方法，但那两个方法不是同一个`Function`的实例。创建两个完成同样任务的`Function`实例的确没有必要。
## 原型模式
我们创建的每个函数都有一个`prototype`（原型）属性，这个属性是一个指针，指向一个对象，而这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。如果按照字面意思来理解，那么`prototype`就是通过调用构造函数而创建的那个对象实例的原型对象。使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法。
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

### 原型模式的问题
它省略了为构造函数传递初始化参数这一环节，结果所有实例在默认情况下都将取得相同的属性值。虽然这会在某种程度上带来一些不方便，但还不是原型的最大问题。原型模式的最大问题是由其共享的本性所导致的。**原型中所有属性是被很多实例共享的，这种共享对于函数非常合适。对于那些包含基本值的属性倒也说得过去，通过在实例上添加一个同名属性，可以隐藏原型中的对应属性。对于包含引用类型值的属性来说，问题就比较突出了。**
```
function Person() {
}
Person.prototype = {
  constructor: Person,
  name: "zxlg",
  age: 24,
  friends: ["whb", "zf"],
  sayName: function () {
    alert(this.name);
  }
};
var person1 = new Person();
var person2 = new Person();
person1.friends.push("cm");
person1.friends; //"whb", "zf", "cm"
person2.friends; //"whb", "zf", "cm"
person1.friends === person2.friends; //true
```
在此，`Person.prototype`对象有一个名为`friends`的属性，该属性包含一个字符串数组。然后，创建了`Person`的两个实例。接着，修改了`person1.friends`引用的数组，向数组中添加了一个字符串。由于`friends`数组存在于`Person.prototype`而非`person1`中，所以刚刚提到的修改也会通过`person2.friends`（与`person1.friends`指向同一个数组）反映出来。假如我们的初衷就是像这样在所有实例中共享一个数组，那么对这个结果我没有话可说。可是，实例一般都是要有属于自己的全部属性的。而这正是单独使用原型模式的问题所在。

## 组合模式
创建自定义类型的最常见方式，就是组合使用构造函数模式与原型模式。构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性。结果，每个实例都会有自己的一份实例属性的副本，但同时又共享着对方法的引用，最大限度地节省了内存。另外，这种混成模式还支持向构造函数传递参数；可谓是集两种模式之长。
```

## 动态原型模式

## 寄生构造函数模式

## 稳妥构造函数模式

## 参考文献
1. Javascript高级程序设计(第三版)