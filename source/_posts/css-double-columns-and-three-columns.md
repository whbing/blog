---
title: CSS定宽+自适应布局
date: 2017-08-22 17:45:25
tags: [css,双飞翼布局，圣杯布局]
categories: [CSS]
---

# 定宽+自适应两列布局
## absolute + margin

```
<div class="container">
    <div class="main">主列</div>
    <div class="sidebar">边栏</div>
</div>
<div class="footer"></div>

```
<!-- more -->

```
.container{
    position: relative;
    /*   overflow: hidden; */
}
.main{
    margin-left:350px;
    height: 500px;
    background-color: rgba(0,255,0,.5);
}
.sidebar{
    position: absolute;
    top: 0;
    left:0;
    width: 300px;
    height:650px;
    background-color: rgba(255,0,0,.5);
}
.footer{
    width : 100%;
    height : 300px;
    margin-top : 50px;
    background-color: rgba(0,0,255,.5);
}
```

优点：
- 支持主列和边栏顺序互换
- 支持主列优先显示，即优先加载渲染

缺点：因为`sidebar`列脱离了文档流，当`sidebar`列比`main`列高时会覆盖后面的布局：[点击查看在线demo](https://codepen.io/zxlg/pen/qXYjGw)。如果在`container`容器上添加`overflow:hidden`就会使`sidebar`溢出部分被裁减。在这种布局方式下，这个问题确实没有有效的解决办法。

## float + margin

```
<div class="sidebar">边栏</div>
<div class="main">主列</div>
```

```
.main{
    margin-left:210px;
    height: 200px;
    background-color: rgba(0,255,0,.5);    
}
.sidebar{
    float: left;
    width: 200px;
    height:250px;
    background-color: rgba(255,0,0,.5);
}
```

[点击查看在线demo](https://codepen.io/zxlg/pen/rzvzXB)
优点：左右两列顺序可互换
缺点：不支持主列优先显示

## float + 负margin(双飞翼布局)

```
<div class="main-wrapper">
    <div class="main">主列</div>
</div>
<div class="sidebar">边栏</div>
```

```
.main-wrapper{
    float:left;
    width:100%;
}
.main{
    margin-left:350px;
    height: 300px;
    background-color: rgba(0,255,0,.5);

}
.sidebar{
    float: left;
    width: 300px;
    height:400px;
    margin-left: -100%;
    background-color: rgba(255,0,0,.5);
}
```

[点击查看在线demo](https://codepen.io/zxlg/pen/qXYPJP)
这个布局即为双飞翼布局，双飞翼布局源自淘宝UED，现在查看下淘宝店铺的DOM结构，就能找到双飞翼布局的身影。实现过程：
1. 首先浮动`main`列和`sidebar`列，然后通过`负margin`正确定位`sidebar`列。
2. 把`main`列嵌套在一个`div`里，该`div`的宽度值设为100%。
3. 最后通过设置`main`列的`margin-left`消除被`sidebar`覆盖的部分即可。

双飞翼布局优点：
1. DOM按照主、子、附加列的顺序加载，实现了重要内容先加载。
2. `main`部分是自适应宽度的，很容易在定宽布局和流体布局中切换。
3. 在浏览器上的兼容性非常好，IE5.5以上都支持。
4. 实现了内容与布局的分离，即Eric提到的Any-Order Columns.
5. 任何一栏都可以是最高栏，不会出问题。
6. 需要的hack非常少。


# 两列定宽+自适应三列布局
## 圣杯布局
圣杯布局源自 Matthew Levine 在06年的一篇[文章](https://alistapart.com/article/holygrail)，其DOM结构如下：
```
<div class="container">
  <div class='main'>主体</div>
  <div class='sub'>边栏</div>
  <div class="extra">另一边栏</div>
</div>

```
### 实现圣杯布局的步骤
1. 首先要使得`main`,`sub`,`extra`三列浮动，然后利用负`margin`将`sub`和`extra`定位到`main`两边
    ```
    .main{
        float:left;
        height:100px;
        width:100%;
        background-color:rgba(255,0,0,.5);
    }
    .sub{
        float:left;
        height:100px;
        width:200px;
        margin-left:-100%;
        background-color:rgba(0,255,0,.5);
    }
    .extra{
        float:left;
        height:100px;
        width:100px;
        margin-left:-100px;
        background-color:rgba(0,0,255,.5);
    }

    ```
2. `sub`和`extra`定位到`main`两边，但是覆盖了`main`列，圣杯布局对`container`容器添加边距，使得`main`列出现在正确的位置
    ```
    .container{
        height:100px;

        /*第二步添加*/
        padding-left:210px;
        padding-right:110px;

    }

    ```
3. 对`container`容器添加内边距虽然让`main`列出现在正确的位置，但是也使得`sub`和`extra`列因为内边距影响偏离了原来的位置。圣杯布局使用相对定位让`sub`和`extra`列出现在正确的位置。
    ```
    .sub{
        float:left;
        height:100px;
        width:200px;
        margin-left:-100%;
        background-color:rgba(0,255,0,.5);

        /*第三步添加*/
        position:relative;
        left:-210px;
    }
    .extra{
        float:left;
        height:100px;
        width:100px;
        margin-left:-100px;
        background-color:rgba(0,0,255,.5);

        /*第三步添加*/
        position:relative;
        right:-110px;
    }

    ```

4. 当浏览器缩小到一定程度时，这个布局可能会被破坏，可以在body上添加`min-width`属性解决。最终的圣杯布局CSS代码如下：
    ```
    body{
        /*第四步添加*/
        min-width:520px; /* 2*sub + extra */
    }
    .container{
        height:100px;

        /*第二步添加*/
        padding-left:210px;
        padding-right:110px;

    }
    .main{
        float:left;
        width:100%;
        height:100px;
        background-color:rgba(255,0,0,.5);
    }
    .sub{
        float:left;
        height:100px;
        width:200px;
        margin-left:-100%;
        background-color:rgba(0,255,0,.5);

        /*第三步添加*/
        position:relative;
        left:-210px;
    }
    .extra{
        float:left;
        height:100px;
        width:100px;
        margin-left:-100px;
        background-color:rgba(0,0,255,.5);

        /*第三步添加*/
        position:relative;
        right:-110px;
    }

    ```
[点击查看在线demo](https://codepen.io/zxlg/pen/qXYyGv?editors=1100#0)

优点：
- 使主要内容列先加载。
- 允许任何列是最高的。
- 没有额外的div。
- 需要的hack很少。

### 和双飞翼布局比较
和圣杯布局一样，分别浮动`main`、`sub`和`extra`列，然后利用负外边距正确定位`sub`和`extra`列。
这时依旧面临和圣杯布局同样的问题：`main`列没有正确定位且被`sub`、`extra`列覆盖。双飞翼布局的解决办法是在`main`列外面包裹了一个宽度100%的`div`，然后通过设置`main`列的左、右外边距正确定位`main`列。

异同点：
1. 俩种布局方式都是把主列放在文档流最前面，使主列优先加载。
2. 两种布局方式在实现上也有相同之处，都是让三列浮动，然后通过负外边距形成三列布局。
3. 两种布局方式的不同之处在于如何处理中间主列的位置：圣杯布局是利用父容器的左、右内边距定位；双飞翼布局是把主列嵌套在div后利用主列的左、右外边距定位。

# 参考文献
1. [浅析圣杯布局和双飞翼布局](https://theqwang.github.io/2016/01/08/%E6%B5%85%E6%9E%90%E5%9C%A3%E6%9D%AF%E5%B8%83%E5%B1%80%E5%92%8C%E5%8F%8C%E9%A3%9E%E7%BF%BC%E5%B8%83%E5%B1%80/)