# AT2396 Infinite Sequence

---

## 题面

[题目链接](https://www.luogu.org/problemnew/show/AT2396)

---

## 题解

**神仙纯思维题**

首先可以发现如果两个不是1数连在一起那么后面的一定全都一样

然后就可以转移了

设$Dp[i]$表示$[i,n]$填$[1,n]$的方案数

那么：

- $a[i]$为1，对$Dp[i]$贡献$Dp[i + 1]$
- $a[i]$不为1，但是后$a[i]$个数都是1，且$i+a[i]$没有超出$n$, 对$Dp[i]$贡献$\sum_{j=i+3}^{n}{Dp[j]}$，用前缀和优化
- $a[i]$不为1，下一个数也不为1，对$Dp[i]$贡献$(n - 1) * (n - 1)$（随便填）
- $a[i] + i$超出或等于$n$，对$Dp[i]$贡献$i + 1$

然后你会发现代码很短

---

## 代码

```c++
/**************************
  * Writer : Leo101
  * Problem : AT2396 Infinite Sequence
  * Tags : Dp，递推
**************************/
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
#define File(s) freopen(#s".in", "r", stdin); freopen(#s".out", "w", stdout)
#define gi get_int()
const int Max_N = 2e6, Mod = 1e9 + 7;
int get_int()
{
	int x = 0, y = 1; char ch = getchar();
	while ((ch < '0' || ch > '9') && ch != '-')
		ch = getchar();
	if (ch == '-') y = -1,ch = getchar();
	while (ch <= '9' && ch >= '0') {
		x = x * 10 + ch - '0';
		ch = getchar();
	}
	return x * y;
}

long long Dp[Max_N];

int main()
{
	File(code);

	int n = gi;
	long long Sum = 0;
	Dp[n] = n; Dp[n - 1] = 1ll * n * n % Mod;
	for (int i = n - 2; i > 0; i--) 
		Sum = (Sum + Dp[i + 3]) % Mod,
		Dp[i] = (Sum + i + 1 + Dp[i + 1] + 1ll * (n - 1) * (n - 1)) % Mod;
	printf("%lld", Dp[1]);

	return 0;
}
```