# Luogu4109 [HEOI2015]定价

---

## 题面

[题目链接](https://www.luogu.org/problemnew/show/P4109)

---

## 题解

每次暴力，从$l$开始算起

每次可以给l直接加$10^Ans$，因为这个区间内没有比他更优秀的了

虽然不是正解但是很优秀

---

## 代码

```c++
/**************************
  * Writer : Leo101
  * Problem : Luogu P4109 [HEOI2015]定价
  * Tags : 贪心，暴力
**************************/
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <cmath>
#define File(s) freopen(#s".in", "r", stdin); freopen(#s".out", "w", stdout)
#define gi get_int()
#define INF 0x3f3f3f3f
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

int main()
{
	File(code);

	int T = gi;
	while (T--) {
		int l = gi, r = gi, Min = INF, Ans = INF;
		while (l <= r) {
			int x = l, Count = 0;
			while (x % 10 == 0) x /= 10, Count++;
			int y = x, Len = 0, f = x % 10;
			while (y != 0) Len++, y /= 10;
			if (f == 5) Len = Len * 2 - 1;
			else Len = Len * 2;
			if (Min > Len) Min = Len, Ans = l;
			l += pow(10, Count);
		}
		
		printf("%d\n", Ans);
	}

	return 0;
}
```