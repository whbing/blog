---
title: 动态规划
date: 2017-08-13 14:03:14
tags:
    - 算法
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
### 分析
状态转移方程：`dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - weight[i - 1]] + value[i - 1])`
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
### 分析
状态转移方程：`dp[i][j] = max(dp[i - 1][j - num * weight[i - 1]] + num * value[i - 1]) (0<=num * weight[i - 1]<=j)`
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
## 多重背包

## 参考文献
1. [背包问题九讲](http://love-oriented.com/pack/)
2. [动态规划之背包问题](http://www.hawstein.com/posts/dp-knapsack.html)