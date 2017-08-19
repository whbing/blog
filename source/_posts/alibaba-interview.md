---
title: 阿里巴巴在线笔试
date: 2017-08-17 18:23:35
tags:
	- js
    - html
    - css
    - interview
categories:
	- Interview
---
# 前端
## 题目
利用面向对象思想完成买家信息删除功能，每一条买家信息包含
- 姓名 (name)
- 性别 (sex)
- 电话号码 (number)
- 省份 (province)

## 要求：
1. 不能借助任何三方库，需使用原生代码实现
2. 结合给出的基本代码结构，在下方2处code here处补充代码，完成买家信息的删除功能，注意此页面需要在手机上清晰显示。
3. js代码可任意调整，例如可使用es6语法完成
<!--more-->
## 代码
```
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <!--code here-->
    <title>demo</title>
    <style>
    * {
        padding: 0;
        margin: 0;
    }

    .head,
    li div {
        display: inline-block;
        width: 70px;
        text-align: center;
    }

    li .id,
    li .sex,
    .id,
    .sex {
        width: 15px;
    }

    li .name,
    .name {
        width: 40px;
    }

    li .tel,
    .tel {
        width: 90px;
    }

    li .del,
    .del {
        width: 15px;
    }

    ul {
        list-style: none;
    }

    .user-delete {
        cursor: pointer;
    }

    @media screen and (max-width: 768px) {
        //宽度小于768px时
        .head,
        li div {
            display: inline-block;
            width: 23.3%;
            text-align: center;
        }

        li .id,
        li .sex,
        .id,
        .sex {
            width: 5%;
        }

        li .name,
        .name {
            width: 13.3%;
        }

        li .tel,
        .tel {
            width: 30%;
        }

        li .del,
        .del {
            width: 5%;
        }

        ul {
            list-style: none;
        }

        .user-delete {
            cursor: pointer;
        }
    }
    </style>
</head>

<body>
    <div id="J_container">
        <div class="record-head">
            <div class="head id">序号</div>
            <div class="head name">姓名</div>
            <div class="head sex">性别</div>
            <div class="head tel">电话号码</div>
            <div class="head province">省份</div>
            <div class="head">操作</div>
        </div>
        <ul id="J_List">
            <li>
                <div class="id">1</div>
                <div class="name">张三</div>
                <div class="sex">男</div>
                <div class="tel">13788888888</div>
                <div class="province">浙江</div>
                <div class="user-delete">删除</div>
            </li>
            <li>
                <div class="id">2</div>
                <div class="name">李四</div>
                <div class="sex">女</div>
                <div class="tel">13788887777</div>
                <div class="province">四川</div>
                <div class="user-delete">删除</div>
            </li>
            <li>
                <div class="id">3</div>
                <div class="name">王二</div>
                <div class="sex">男</div>
                <div class="tel">13788889999</div>
                <div class="province">广东</div>
                <div class="user-delete">删除</div>
            </li>
        </ul>
    </div>
    <script>
    var ul = document.getElementById("J_List");
    var del = document.getElementsByClassName('user-delete');
    for (let i = 0; i < del.length; i++) {
        del[i].onclick = function(event) {
            ul.removeChild(event.target.parentNode);
        };
    }
    </script>
</body>

</html>
```

# 测试
## 题目
菜鸟网络仓库有一排小货架，共有N个，货架的底部是空的，现在智能机器人在某个货架下，小明写了一个非常简单的智能机器人移动程序,逻辑如下：每过1分钟，智能机器人必须随机的从一个货架下移动到相邻的一个货架下。比如刚开始智能机器人在第4个货架下，过1分钟后，智能机器人可能会在第3个货架下或者在第5个货架下。如果刚开始时智能机器人在第1个货架下，过1分钟以后，智能机器人一定会在第2个货架下。
现在告诉你货架的数目N，已经智能机器人开始所在的位置P，小明很想知道，在M分钟后，智能机器人到达第T货架，一共有多少种行走方案。请帮小明算一算。
输入：
N
P
M
T
输出：一共有多少种行走方案
## 代码
```
// java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		while (sc.hasNext()) {
			int N = sc.nextInt();// 货架数量
			int P = sc.nextInt();// 起始位置
			int M = sc.nextInt();// 时间
			int T = sc.nextInt();// 结束位置
			int res = solution(N, P, M, T);
			System.out.println(res);
		}

	}

	public static int solution(int n, int p, int m, int t) {
		int[][] dp = new int[m + 1][n + 2];// 起始末尾添加两列，防止越界
		dp[0][p] = 1;
		for (int i = 1; i < dp.length; i++) { // 时间从1开始，dp[0][p]为1，其他均为0
			for (int j = 1; j < dp[0].length - 1; j++) {// 位置从1开始到length-2是为了防止数组越界
				dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j + 1];
			}
		}
		return dp[m][t];
	}
}
```

```
// js

var readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});
var length = 4;
var flag = 0;
var arr = [];
rl.on('line', function (input) {
    flag++;
    input = parseInt(input);
    arr.push(input);
    if (flag == length) {
        var res = soulution(arr);
        console.log(res);
        flag = 0;
        arr = [];
    }
}).on('close', function () {

});

function soulution(arr) {
    var n = arr[0];//货架数量
    var p = arr[1];//初始位置
    var m = arr[2];//经过时间
    var t = arr[3];//结束位置
    var dp = [];
    for (var i = 0; i < m + 1; i++) {
        dp[i] = new Array;
        for (var j = 0; j < n + 2; j++) {
            dp[i][j] = 0;
        }
    }
    dp[0][p] = 1;
    for (var i = 1; i < dp.length; i++) {
        for (var j = 1; j < dp[0].length - 1; j++) { //1-n防止数组越界
            dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j + 1];
        }
    }
    return dp[m][t];
}
```