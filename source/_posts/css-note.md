---
title: 精通CSS笔记与心得
date: 2017-08-22 15:47:09
tags: [css,前端,读书笔记]
categories: [CSS]
---

# 基础知识
## 文档类型
## DOCTYP切换
## 浏览器模式
<!-- more -->
# CSS选择器
## 常用选择器
类型选择器
后代选择器
ID选择器
类选择器
伪类(伪类连接)
## 通用选择器
## 高级选择器
### 子选择器和通报选择器
### 属性选择器

## 层叠和特殊性
**层叠次序：**
1. 标有`important`的用户样式。
2. 标有`important`的作者样式。
3. 作者样式
4. 用户样式
5. 浏览器/用户代理的样式
然后根据选择器的特殊性决定规则的次序，具有更特殊选择器的规则优于具有一般选择器的规则。如果两个规则的特殊性相同，那么后定义的规则优先。
**特殊性的四个等级：**
1. 行内样式
2. ID选择器的总数
3. 类，伪类，属性选择器的总数
4. 类型选择器和伪类选择器的数量

## HTML中引入CSS
有 4 种方式可以在 HTML 中引入 CSS。其中有 2 种方式是在 HTML 文件中直接添加 CSS 代码，另外两种是引入外部 CSS 文件。
### 内联方式
内联方式指的是直接在 HTML 标签中的`style`属性中添加 CSS。
示例：
`
<div style="background: red"></div>
`
这通常是个很糟糕的书写方式，它只能改变当前标签的样式，如果想要多个`<div>`拥有相同的样式，你不得不重复地为每个 `<div>`添加相同的样式，如果想要修改一种样式，又不得不修改所有的`style`中的代码。很显然，内联方式引入`CSS`代码会导致 HTML 代码变得冗长，且使得网页难以维护。

### 嵌入方式
嵌入方式指的是在 HTML 头部`<head>`中的`<style>`标签下书写 CSS 代码。
示例：
```
<head>
    <style>

    .content {
        background: red;
    }

    </style>
</head>
```
嵌入方式的 CSS 只对当前的网页有效。因为 CSS 代码是在 HTML 文件中，所以会使得代码比较集中，当我们写模板网页时这通常比较有利。因为查看模板代码的人可以一目了然地查看 HTML 结构和 CSS 样式。因为嵌入的 CSS 只对当前页面有效，所以当多个页面需要引入相同的 CSS 代码时，这样写会导致代码冗余，也不利于维护。

### 链接方式
链接方式指的是使用 HTML 头部的`<head>`标签中通过`<link>`标签引入外部的 CSS 文件。
示例：
```
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
```
这是最常见的也是最推荐的引入 CSS 的方式。使用这种方式，所有的 CSS 代码只存在于单独的 CSS 文件中，所以具有良好的可维护性。并且所有的 CSS 代码只存在于 CSS 文件中，CSS 文件会在第一次加载时引入，以后切换页面时只需加载 HTML 文件即可。

### 导入方式
导入方式指的是使用 CSS 规则引入外部 CSS 文件。
示例：
```
<style>
    @import url(style.css);
</style>
```
### 比较链接方式和导入方式
链接方式（下面用 link 代替）和导入方式（下面用 @import 代替）都是引入外部的 CSS 文件的方式
**link 属于 HTML，通过`<link>`标签中的`href`属性来引入外部文件，而 @import 属于 CSS，所以导入语句应写在 CSS 中，要注意的是导入语句应写在样式表的开头，否则无法正确导入外部文件；**
@import 是 CSS2.1 才出现的概念，所以如果浏览器版本较低，无法正确导入外部样式文件；
**当 HTML 文件被加载时，link 引用的文件会同时被加载，而 @import 引用的文件则会等页面全部下载完毕再被加载；**
综上不推荐使用 @import。

## 删除注释和优化样式表
脚本删除注释
CSS压缩

# CSS盒子模型
## W3C盒模型和IE盒子模型
W3C盒子 = 内容 + 内边距 + 边框 + 外边距
IE盒子 = 内容 + 外边距（IE的内容包含了内边距和边框）
CSS中`box-sizing`定义使用何种盒子模型



# 参考文献
1. [HTML 中引入 CSS 的方式](https://segmentfault.com/a/1190000003866058)