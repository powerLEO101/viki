# AT2567 RGB Sequence

---

## 题面

[题目链接](https://www.luogu.org/problemnew/show/AT2567)

---

## 题解

设$Dp[i][j][k]$为位置$i,j,k$上分别是三种不同的颜色的方案数

转移就很简单了，若不考虑其他位置，只看这三个位置，看一下是否满足输入的限制

如果满足限制，那么就用与扩展其他状态，具体是：

1. $Dp[max{i, j, k} + 1][j][k] += Dp[i][j][k]$

2. $Dp[i][max(i, j, k) + 1][k] += Dp[i][j][k]$

3. $Dp[i][j][max(i, j, k) + 1] += Dp[i][j][k]$

当$max(i, j, k) == n$时，可以计入答案

---

## 代码

```c++
/**************************
  * Writer : Leo101
  * Problem : AT2567 RGB Sequence
  * Tags : Dp
**************************/
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <vector>
#define File(s) freopen(#s".in", "r", stdin); freopen(#s".out", "w", stdout)
#define gi get_int()
const int MOD = 1e9 + 7, Max_N = 302;
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

int Dp[Max_N][Max_N][Max_N], l[Max_N], r[Max_N], x[Max_N];
std :: vector<int> Lim[Max_N];

int main()
{
	File(code);

	int n = gi, m = gi;
	for (int i = 1; i <= m; i++) {
		l[i] = gi, r[i] = gi, x[i] = gi;
		Lim[r[i]].push_back(i);
	}

	Dp[0][0][0] = 1;

	long long Ans = 0;

	for (int i = 0; i <= n; i++) {
		for (int j = 0; j <= n; j++) {
			if (i == j && j != 0) continue;
			for (int k = 0; k <= n; k++) {
				if (k != 0 && (i == k || j == k)) continue;
				int Max = std :: max({i, j, k});
				bool Flag = false;
				for (auto it = Lim[Max].begin(); it != Lim[Max].end(); it++) {
					int Count = 0;
					if (l[*it] <= i) Count++;
					if (l[*it] <= j) Count++;
					if (l[*it] <= k) Count++;
					if (Count != x[*it]) {
						Flag = true;
						break;
					}
				}
				if (Flag == true) continue;
				Dp[Max + 1][j][k] = (Dp[Max + 1][j][k] + Dp[i][j][k]) % MOD;
				Dp[i][Max + 1][k] = (Dp[i][Max + 1][k] + Dp[i][j][k]) % MOD;
				Dp[i][j][Max + 1] = (Dp[i][j][Max + 1] + Dp[i][j][k]) % MOD;
				if (Max == n) Ans = (Ans + Dp[i][j][k]) % MOD;
			}
		}
	}

	printf("%lld", Ans);

	return 0;
}
```