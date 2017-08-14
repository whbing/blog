---
title: Javascript操作DOM总结
date: 2017-08-07 11:11:59
tags:
	- js
categories:
	- Javascript
---

## Node.innerText
Node.innerText 是一个表示一个节点及其后代的“渲染”文本内容的属性。

作为一个获取器，如果用光标突出显示元素的内容，然后将其复制到剪贴板，则它将近似于用户将获得的文本。此功能最初由Internet Explorer引入，并在所有主要浏览器供应商采用后于2016年在HTML标准中正式规定。

`Node.textContent`是一个有点类似的替代方案，虽然两者之间有重要的区别。

## element.innerHTML
`Element.innerHTML`属性设置或获取描述元素后代的HTML语句。

Note: 如果一个`<div>`,` <span>`, 或 `<noembed>`节点具有一个文本子节点,包含字符`(&)`,` (<)`,  或`(>)`, `innerHTML`将这些字符分别返回为`＆amp;`, `＆lt;`和`＆gt;`。使用`Node.textContent`获取一个这些文本节点内容的正确副本。


