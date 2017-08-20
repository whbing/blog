---
title: Markdown技巧
date: 2017-08-04 10:11:44
tags: [Markdown]
categories: 
    - Markdown
---
# 一、代码
## 代码区块
- 第一种，只要简单地缩进 4 个空格或是 1 个制表符就可以
- 第二种，在代码段落的头部和尾部用\`\`\`包围起来

## 行内代码块
用\` \`包围

# 二、链接
1. 文字链接 
```
[链接名称](http://链接网址)
```

2. 网址链接 
```
<http://链接网址>
```

# 三、图片
1. 行内式
```
![alt图片名称](http://图片网址)
```
## 设置图片居中
```
<div align='center'>
![alt图片名称](http://图片网址)
<p>图片名</p>
</div>
```
2. 参考式
```
![alt图片名称][本地图片地址]
```

目前为止Markdown 还没有办法指定图片的宽高，如果需要，可以使用普通的`<img>`标签。
<!--more-->
# 四、表格
## 单元格和表头
使用`|`来分隔不同的单元格，使用`-`来分隔表头和其他行：
```
name | age
---- | ---
LearnShare | 12
Mike |  32
```
name | age
---- | ---
LearnShare | 12
Mike |  32

## 对齐
在表头下方的分隔线标记中加入`:`，即可标记下方单元格内容的对齐方式：

`:---`代表左对齐
`:--:`代表居中对齐
`---:`代表右对齐
```
| left | center | right |
| :--- | :----: | ----: |
| aaaa | bbbbbb | ccccc |
| a    | b      | c     |
```
| left | center | right |
| :--- | :----: | ----: |
| aaaa | bbbbbb | ccccc |
| a    | b      | c     |

# 五、内嵌HTML
不在 Markdown 涵盖范围之内的标签，都可以直接在文档里面用 HTML 撰写。不需要额外标注这是 HTML 或是 Markdown，只要直接加标签就可以了。

注意 HTML 区块元素，比如 `<div>`、`<table>`、`<pre>`、`<p>` 等标签，必须在前后加上空行与其它内容区隔开，还要求它们的开始标签与结尾标签不能用制表符或空格来缩进。Markdown 的生成器有足够智能，不会在 HTML 区块标签外加上不必要的 `<p> `标签。
# 六、公式
如果想要在Markdown文档中显示一个公式就需要先插入下面一句话，这实际上是插入了一个图片。
```
![公式名](http://latex.codecogs.com/png.latex?这里输入您的公式)
```

上面这句话是插入一个png图片格式的公式，而下面这句话则是插入gif图片格式的公式。
```
![公式名](http://latex.codecogs.com/png.latex?这里输入您的公式)
```

# 七、转义
Markdown符号用反斜杠\\转义

______________
2017.08.15更新
______________

# 八、横线
在一行中用三个以上的星号(*)、减号(-)、下划线(_)来建立一个分隔线；除空格外行内不能有其他字符；（除第一个符号的左侧最多添加三个空格外）三个相同符号两侧可以添加任意多个空格。

# 参考文献
1. [Markdown教程](https://kennylee26.gitbooks.io/markdown/content/grammar/Inline-HTML.html)
2. [Markdown 编辑器语法指南](https://segmentfault.com/markdown#articleHeader7)
