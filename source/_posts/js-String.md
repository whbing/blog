---
title: Javascript中String对象总结
date: 2017-08-03 11:00:43
tags: [js]
categories: 
    - Javascript
---

_________________________________
2017.08.20更新
_________________________________
## String.prototype.match()
### 语法
```
str.match(regexp);
```
### 参数
`regexp`
一个正则表达式对象。如果传入一个非正则表达式对象，则会隐式地使用`new RegExp(obj)`将其转换为一个`RegExp`。如果你未提供任何参数，直接使用`match()`，那么你会得到一个包含空字符串的`Array ：[""]`。
### 返回值
`array`
一个包含了整个匹配结果以及任何括号捕获的匹配结果的`Array`；如果没有匹配项，则返回`null`。

### 描述
如果正则表达式没有`g`标志，则`str.match()`会返回和`RegExp.exec()`相同的结果。而且返回的`Array`拥有一个额外的`input`性，该属性包含被解析的原始字符串。另外，还拥有一个`index`属性，该属性表示匹配结果在原字符串中的索引（以0开始）。
如果正则表达式包含`g`标志，则该方法返回一个`Array`，它包含所有匹配的子字符串而不是匹配对象。捕获组不会被返回(即不返回`index`属性和`input`属性)。如果没有匹配到，则返回`null`。

### 注意
如果你需要知道一个字符串是否匹配一个正则表达式`RegExp`，可使用`search()`。
如果你只是需要第一个匹配结果，你可能想要使用`RegExp.exec()`。
如果你想要获得捕获组，并且设置了全局标志，你需要用`RegExp.exec()`。

## String.prototype.replace()
### 语法
```
str.replace(regexp|substr,newSubstr|function)
```
### 参数
`regexp` (pattern)
一个 正则表达式对象或者其字面量。该正则所匹配的内容会被第二个参数的返回值替换掉。
`substr` (pattern)
一个要被`newSubStr`替换的字符串。其被视为一整个字符串，而不是一个正则表达式。仅仅是第一个匹配会被替换。
`newSubStr`(replacement)
用于替换掉第一个参数在原字符串中的匹配部分的 字符串。该字符串中可以内插一些特殊的变量名。参考下面的使用字符串作为参数。
`function`(replacement)
一个用来创建新子字符串的函数，该函数的返回值将替换掉第一个参数匹配到的结果。参考下面的指定一个函数作为参数。
在这种情况下，当匹配执行后， 该函数就会执行。 函数的返回值作为替换字符串。 (注意:  上面提到的特殊替换参数在这里不能被使用。) 另外要注意的是， 如果第一个参数是正则表达式， 并且其为全局匹配模式， 那么这个方法将被多次调用， 每次匹配都会被调用。
### function参数
`match`
匹配的子串。(对应于上述的$&。)
`p1,p2, ...	`
假如`replace()`方法的第一个参数是一个RegExp 对象，则代表第n个括号匹配的字符串。（对应于上述的$1，$2等。）
`offset`
匹配到的子字符串在原字符串中的偏移量。（比如，如果原字符串是“abcd”，匹配到的子字符串是“bc”，那么这个参数将是1）
`string`
被匹配的原字符串。
```
function replacer(match, p1, p2, p3, offset, string) {
  // p1 is nondigits, p2 digits, and p3 non-alphanumerics
  return [p1, p2, p3].join(' - ');
}
var newString = 'abc12345#$*%'.replace(/([^\d]*)(\d*)([^\w]*)/, replacer);//'abc - 12345 - #$*%'
```
### 返回值
一个部分或全部匹配由替代模式所取代的新的字符串。

## 参考文献
1. [MDN-String](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)