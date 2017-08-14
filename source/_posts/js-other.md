---
title: 牛客网做题Javascript笔记
date: 2017-08-06 22:30:57
tags:
	- js
categories:
	- Javascript
---


1. JavaScript 中包含以下 7 个全局函数，用于完成一些常用的功能：escape( )、eval( )、isFinite( )、isNaN( )、parseFloat( )、parseInt( )、unescape( )。

2. JavaScript为指定元素绑定一个事件处理器函数

3. Javascript块内声明函数
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
4. Jquery获取宽高
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
5. 浏览器兼容性问题
- SD9017: Firefox 不支持 DOM 对象的 outerHTML、innerText、outerText 属性(参见<http://w3help.org/zh-cn/causes/SD9017>)
- SD9010: 仅 IE 中的 createElement 方法支持传入 HTML String 做参数
- SD9006: IE 混淆了 DOM 对象属性（property）及 HTML 标签属性（attribute），造成了对 setAttribute、getAttribute 的不正确实现