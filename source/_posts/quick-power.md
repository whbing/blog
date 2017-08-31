---
title: 求幂的高效算法
date: 2017-08-30 10:59:02
tags: [快速幂,二分快速幂，矩阵快速幂,快速乘,算法]
categories: [算法]
---

求解`a^n`次方，正常求解需要O(n)时间，若n很大，则需要更快速的算法

## 快速幂
基础：任何一个数都可以用二进制表示
若需求`3^11`，可将指数进行一个`logN`的变换，`11 = 2^3 + 2^1 + 2^0 `，只需算`3^8,3^2,3^1`，再将其相乘。

<!-- more -->
```java
/**
 * @author zxlg
 *
 */
import java.math.*;
public class Quick_power {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		BigInteger res = quick_power(2,12);
		System.out.println(res);
	}

	// 快速幂
	public static BigInteger quick_power(int num, int power) {
		BigInteger ans = BigInteger.valueOf(1);
		BigInteger base = BigInteger.valueOf(num);
		while (power != 0) {
			// power转为2进制，若该位为1
			if ((power & 1) == 1) {
				ans = ans.multiply(base);
			}
			// 转换为基础值，没轮循环为原值的平方
			base = base.multiply(base);
			// power取下一位
			power = power >> 1;
		}
		return ans;
	}
}
```

## 快速幂取模

利用了快速幂的思想，但取模时有公式：
```
a ^ b % c = (a % c)^b % c
(a * b) % c=(a % c)*(b % c) % c  
```

```java
public class Quick_power_mod {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		long res = quick_power_mod(2, 15, 100000);
		System.out.println(res);
	}

	// 公式：a^b mod c = (a mod c)^c mod c
	// b可以转为2进制计算，降低复杂度
	public static long quick_power_mod(long num, long base, long mod) {
		long ans = 1;
		num = num % mod;
		while (base != 0) {
			if ((base & 1) == 1) {
				ans = ans * num % mod;
			}
			num = num * num % mod;
			base >>= 1;
		}
		return ans;
	}

}

```
## 二分快速幂

## 矩阵快速幂

## 快速乘


## 参考文献
1. [二分幂，快速幂，矩阵快速幂，快速乘](http://blog.csdn.net/mosbest/article/details/69264953)
2. [快速幂取模算法](http://www.cnblogs.com/wuyudong/p/3637479.html)
3. [快速幂取模算法详解](http://blog.csdn.net/ltyqljhwcm/article/details/53043646)
