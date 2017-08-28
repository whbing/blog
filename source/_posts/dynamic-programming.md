---
title: 动态规划
date: 2017-08-13 14:03:14
tags: [算法,动态规划]
categories: 
    - 算法
---

______________
2017.08.17更新
______________

# 背包问题
## 01背包
### 状态
`dp[i][j]`表示前i个物品装到剩余容量为j时的最大价值
### 状态转移方程
`dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - weight[i - 1]] + value[i - 1])`
第`i`个物品装或者不装入背包，不装价值为`dp[i-1][j]`,装表示剩下`i-1`个物品装入`j-weight[i-1]`重的最大价值，加上`value[i-1]`
注意讨论前`i`个物品装入背包的时候， 其实是在考查第`i-1`个物品装不装入背包（因为物品是从0开始编号的）
<!--more-->
### 代码
```
public class Pack_01 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
//		int n = 5;// 物品个数
//		int[] value = { 48, 40, 12, 8, 7 };// 物品价值
//		int[] weight = { 6, 5, 2, 1, 1 };// 物品重量
//		int capacity = 8;// 背包容量

		int n = 4;
		int[] value = { 10, 40, 30, 50 };
		int[] weight = { 5, 4, 6, 3 };
		int capacity = 12;
		int res = pack_01(n, capacity, weight, value);
		System.out.println(res);

	}

	public static int pack_01(int n, int capacity, int[] weight, int[] value) {
		int[][] dp = new int[n + 1][capacity + 1];
		for (int i = 1; i < dp.length; i++) {//
			for (int j = 1; j < dp[0].length; j++) {
				if (j >= weight[i - 1]) {
					dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - weight[i - 1]] + value[i - 1]);
				} else {
					dp[i][j] = dp[i - 1][j];// i只与i-1有关
				}
			}
		}
		return dp[n][capacity];
	}

}
```
## 完全背包
### 状态
`dp[i][j]`表示前i个物品装到剩余容量为j时的最大价值
### 状态转移方程
`dp[i][j] = max(dp[i - 1][j - num * weight[i - 1]] + num * value[i - 1]) (0<=num * weight[i - 1]<=j)`
### 代码
```
public class Pack_full {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		int n = 5;// 物品个数
		int[] value = { 48, 40, 12, 8, 7 };// 物品价值
		int[] weight = { 6, 5, 2, 1, 1 };// 物品重量
		int capacity = 8;// 背包容量

		// int n = 4;
		// int[] value = { 10, 5, 30, 40 };
		// int[] weight = { 5, 4, 6, 3 };
		// int capacity = 12;
		int res = pack_full(n, capacity, weight, value);
		System.out.println(res);
	}

	public static int pack_full(int n, int capacity, int[] weight, int[] value) {
		int[][] dp = new int[n + 1][capacity + 1];
		for (int i = 1; i < dp.length; i++) {
			for (int j = 1; j < dp[0].length; j++) {
				for (int num = 0; j >= num * weight[i - 1]; num++) {
					dp[i][j] = Math.max(dp[i][j], dp[i - 1][j - num * weight[i - 1]] + num * value[i - 1]);
				}
			}
		}
		return dp[n][capacity];
	}
}
```
______________
2017.08.18更新
______________

## 硬币找零(方案数)
### 状态
`dp[i][j`]表示使用`changes[0-i]`硬币兑换`j`元的方法总数
### 分析
使用i=0的钱币兑换，只有changes[0]的整数倍的金额才能有1种方法
`dp[0][j * changes[0]] = 1`
### 状态转移方程
不装入第i种钱币，即使用0~i-1种钱币组成j的方法数；装入i钱币，使用0~i的钱币组成j-changes[i]金额的方法数
`dp[i][j] = dp[i - 1][j] +  dp[i][j - changes[i]]`
### 代码
```
//链接：https://www.nowcoder.com/questionTerminal/185dc37412de446bbfff6bd21e4356ec
//来源：牛客网
//
//有一个数组changes，changes中所有的值都为正数且不重复。每个值代表一种面值的货币，每种面值的货币可以使用任意张，对于一个给定值x，请设计一个高效算法，计算组成这个值的方案数。
//给定一个int数组changes，代表所以零钱，同时给定它的大小n，另外给定一个正整数x，请返回组成x的方案数，保证n小于等于100且x小于等于10000。
//测试样例：
//[5,10,25,1],4,15
//返回：
//6

public class ChangeMoney {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		int[] changes = { 5, 10, 25, 1 };
		int n = 4;
		int money = 15;
		int res = change(changes, n, money);
		System.out.println(res);

	}

	public static int change(int[] changes, int n, int money) {
		int[][] dp = new int[n][money + 1];// dp[i][j]表示使用changes[0-i]硬币兑换j元的方法总数。
		for (int i = 0; i < n; i++) { // j=0表示钱为0，组成0元的方法数为1
			dp[i][0] = 1;
		}
		for (int j = 0; j * changes[0] < money + 1; j++) {// 使用i=0的钱币兑换，只有changes[0]的整数倍的金额才能有1种方法
			dp[0][j * changes[0]] = 1;
		}
		// 填表
		for (int i = 1; i < dp.length; i++) {// 数组长度1~n-1在changes数组中有效
			for (int j = 1; j < dp[0].length; j++) {
				// 不装入第i种钱币，即使用0~i-1种钱币组成j的方法数；装入i钱币，使用0~i的钱币组成j-changes[i]金额的方法数
				dp[i][j] = dp[i - 1][j] + (j - changes[i] >= 0 ? dp[i][j - changes[i]] : 0);
			}
		}
		return dp[n - 1][money];
	}
}
```
## 硬币找零(最少硬币数)
## 多重背包

____________________________
2017-08-28更新
____________________________
## 最长递增子序列
### 动态规划法
```javascript
//普通dp算法，复杂度为n^2
function longest_increasing_subsequence_1(arr) {
    //时间复杂度为n^2
    let dp = [];
    for (let i = 0; i < arr.length; i++) {
        dp[i] = 1;
    }
    let max = 0;
    for (let i = 0; i < arr.length; i++) {
        for (let j = 0; j < i; j++) {
            if (arr[j] < arr[i]) {
                dp[i] = Math.max(dp[j] + 1, dp[i]);
            }
        }
        max = Math.max(max, dp[i]);
    }
    return max;
}

```

### 改进的二分查找法

```javascript
//二分查找，复杂度为nlogn
function longest_increasing_subsequence_2(arr) {
    //时间复杂度为nlogn
	//minNum存储的是长度为 index+1 的最小末尾值
	//当arr[i]比minNum数组的最后一个数还大时
	//minNum数组长度加1,其最小末尾为arr[i]
	//这样保证minNum数组的值是有序排列的
	//当arr[i]比比minNum数组的最后一个数小时,
	//对minNum有序数组进行二分查找
	//并将最终找到的右边界返回，将其值改为arr[i]
	//保证了minNum数组存储的一直为最小末尾
	//即相同长度的子序列，选择末尾较小的填入
	//以保证minNum数组一直有序
    let minNum = [];
    for (let i = 0; i < arr.length; i++) {
        length = minNum.length;
        if (arr[i] > minNum[length - 1] || minNum[length - 1] == undefined) {
            minNum[length] = arr[i];
        } else {
            //二分查找
            let change_index = binary_search(minNum,arr[i]);
            minNum[change_index] = arr[i];
        }
    }
    return minNum.length;
}

function binary_search(arr,search_num){
    let seek_left = 0;
    let seek_right = arr.length - 1;
    let half_length = Math.floor((seek_left + seek_right) / 2);
    while ((seek_right - seek_left) > 1) {
        if (arr[half_length] < search_num) {
            seek_left = Math.floor((seek_left + seek_right) / 2);
        } else if (arr[half_length] > search_num) {
            seek_right = Math.floor((seek_left + seek_right) / 2);
        } else {
            seek_right = half_length;
            break;
        }
        half_length = Math.floor((seek_left + seek_right) / 2);
    }
    return seek_right;
}

```


## 最长公共子序列
## 地牢游戏
```javascript


```
## 参考文献
1. [背包问题九讲](http://love-oriented.com/pack/)
2. [动态规划之背包问题](http://www.hawstein.com/posts/dp-knapsack.html)