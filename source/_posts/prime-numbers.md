---
title: 找出100万以内的质数
date: 2017-08-30 10:20:42
tags: [素数,数论,Mfiller-Rabin算法,算法]
categories: [算法]
---

## 暴力测试
最简单的算法，从2枚举到`sqrt(n)`就可以知道是不是素数了。
## 费马小定理
对于质数n和任意整数a，有`a^n ≡ a(mod n)`(同余)。
反之，若满足`a^n≡a(mod n)`，`n`也有很大概率为质数，称`n`是一个基于`a`的伪素数。将两边同时约去一个`n`，则有`a^(n−1)≡1(mod n)`。

这个定理是来判断一个数字是不是合数的，而不是素数。如果不符费马小定理，一定是合数， 如果符合费马小定理不一定是素数（但是是素数的可能性比较高）。

如果判定`n`为合数，那么结果一定正确。如果判定`n`为质数，那么只有当`n`是基于2的伪素数时才会出错

<!-- more -->

## Mfiller-Rabin素数判定
在费马小定理基础上增加了二次判定：
如果`n`是奇素数，则` x^2 ≡ 1(mod n)`的解为`x≡1`或`x ≡ n−1(mod n)`


这个x是：`a^(p−1)=a^(2u∗r)`，所以`p−1=2u∗r`，那么x自然也知道了：这个x是一系列的数字，让在指数`2^u`里拿出一个2就编程`2^(u-1) * 2`，这样不就有平方了么，剩下的底数就是x了，并且在还可以继续不停的这样操作，到` u = 0`为止。

Mfiller-Rabin素数仍会出错：如果是素数，不出错，如果为合数，也会小概率出错`2^(-s)`，所以多次检测，会让错误率变低，这里`s`是检测次数。

## 举例说明
>举个Matrix67 Blog上的例子，假设n=341，我们选取的a=2。则第一次测试时，2^340 mod 341=1。由于340是偶数，因此我们检查2^170，得到2^170 mod 341=1，满足二次探测定理。同时由于170还是偶数，因此我们进一步检查2^85 mod 341=32。此时不满足二次探测定理，因此可以判定341不为质数。

## 代码实现
参考[算法基础 - 素数判定(Miller-Rabin算法)](http://blog.csdn.net/alps1992/article/details/51588971)
```
伪代码：
Miller-Rabin(n):
    If (n <= 2) Then
        If (n == 2) Then
            Return True
        End If
        Return False
    End If

    If (n mod 2 == 0) Then
        // n为非2的偶数，直接返回合数
        Return False
    End If

    // 我们先找到的最小的a^u，再逐步扩大到a^(n-1)

    u = n - 1; // u表示指数
    while (u % 2 == 0) 
        u = u / 2
    End While // 提取因子2

    For i = 1 .. S // S为设定的测试次数
        a = rand_Number(2, n - 1) // 随机获取一个2~n-1的数a
        x = a^u % n
        tu = u
        While (tu < n) 
            // 依次次检查每一个相邻的 a^u, a^2u, a^4u, ... a^(2^k*u)是否满足二次探测定理
            y = x^2 % n 
            If (y == 1 and x != 1 and x != n - 1)   // 二次探测定理
                // 若y = x^2 ≡ 1(mod n)
                // 但是 x != 1 且 x != n-1
                Return False
            End If
            x = y
            tu = tu * 2 
        End While
        If (x != 1) Then    // Fermat测试
            Return False
        End If
    End For
    Return True
```

Java代码编写：
```java
import java.math.*;
import java.util.*;

/**
 * @author zxlg
 *
 */
public class All_primes_in_million {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		print_all_prime_in_million(10000);

	}

	public static void print_all_prime_in_million(int n) {
		ArrayList<Integer> arr = new ArrayList<Integer>();
		for (int i = 0; i < n; i++) {
			if (Miller_Rabin(i)) {
				arr.add(i);
			}
		}
		System.out.println(arr.toString());
		System.out.println(n + "以内共有" + arr.size() + "个素数");
	}

	public static boolean Miller_Rabin(int n) {
		// 小于2的情况
		if (n <= 2) {
			if (n == 2) {
				return true;
			}
			return false;
		}
		// 偶数
		if (n % 2 == 0) {
			return false;
		}

		// 先找到的最小的a^u，再逐步扩大到a^(n-1)

		int u = n - 1;// u表示指数
		int S = 20;// 检测次数

		// 提取公因子2
		while (u % 2 == 0) {
			u /= 2;
		}

		for (int i = 0; i < S; i++) {
			// 随机选取2~n-1之间的数a
			int a = (int) (Math.random() * 2 + (n - 3));
			// x = a^u % n
			int x = quick_power(a, u).mod(BigInteger.valueOf(n)).intValue();
			int tu = u;
			// 依次次检查每一个相邻的 a^u, a^2u, a^4u, ... a^(2^k*u)是否满足二次探测定理
			while (tu < n) {
				int y = x * x % n;
				if (y == 1 && x != 1 && x != n - 1) {// 二次探测定理
					// 若y = x^2 ≡ 1(mod n)
					// 但是 x != 1 且 x != n-1
					return false;
				}
				x = y;
				tu = tu * 2;
			}

			if (x != 1) { // 费马测试
				return false;
			}
		}

		return true;
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

## 参考文献
1. [算法基础 - 素数判定(Miller-Rabin算法)](http://blog.csdn.net/alps1992/article/details/51588971)
