---
title: HTML笔记
date: 2017-08-20 21:30:13
tags: [html,前端]
categories: [HTML]
---

_____________________________
2017.08.20更新
_____________________________
## 锚点链接
### HTML链接 - name 属性
> name 属性规定锚（anchor）的名称。
> 使用 name 属性创建 HTML 页面中的书签。书签不会以任何特殊方式显示，它对读者是不可见的。
> 当使用命名锚（named anchors）时，我们可以创建直接跳至该命名锚（比如页面中某个小节）的链接，这样使用者就无需不> 停地滚动页面来寻找他们需要的信息了。

### 语法
```
<a name="label">锚（显示在页面上的文本）</a>
```
**可以使用 id 属性来替代 name 属性，命名锚同样有效。**

创建:
```
<a name="tips">基本的注意事项 - 有用的提示</a>
```
使用：
```
<a href="#tips">有用的提示</a>
```
在其他页面中创建指向该锚的链接：
```
<a href="http://www.w3school.com.cn/html/html_links.asp#tips">有用的提示</a>
```
将`#`符号和锚名称添加到URL的末端，就可以链接到`tips`这个锚。

## 参考文献
1. [HTML 链接](http://www.w3school.com.cn/html/html_links.asp)