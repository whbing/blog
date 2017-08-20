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