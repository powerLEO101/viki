# Luogu4107 [HEOI2015]兔子与樱花

---

## 题面

[题目链接](https://www.luogu.org/problemnew/show/P4107)

---

## 题解

发现从叶子往Root能删就删一定是最优的（当然还是要排序的）

因为没有后效性

---

## 代码

```c++
/**************************
  * Writer : Leo101
  * Problem : Luogu P4107 [HEOI2015]兔子与樱花
  * Tags : 贪心，Dfs
**************************/
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
#define File(s) freopen(#s".in", "r", stdin); freopen(#s".out", "w", stdout)
#define gi get_int()
#define for_edge(i, x) for (int i = Head[x]; i != -1; i = Edges[i].Next)
const int Max_N = 2100000;
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

int l[Max_N], r[Max_N], Son[Max_N], Cost[Max_N];
int Ans, Cnt, n, m;

bool Cmp(const int& a, const int& b)
{ return Cost[a] < Cost[b]; }
void Dfs(int Now = 0)
{
	for (int i = l[Now]; i < r[Now]; i++) {
		Dfs(Son[i]); Cost[Now]++;
	}

	std :: sort(Son + l[Now], Son + r[Now], Cmp);

	for (int i = l[Now]; i < r[Now]; i++) {
		int To = Son[i];
		if (Cost[Now] + Cost[To] - 1 > m) break;
		Cost[Now] += Cost[To] - 1; Ans++;
	}
}

int main()
{
	File(code);

	n = gi, m = gi;
	for (int i = 0; i < n; i++) Cost[i] = gi;
	for (int i = 0; i < n; i++) {
		int Number = gi;
		l[i] = Cnt;
		for (int j = 0; j < Number; j++) Son[Cnt++] = gi;
		r[i] = Cnt;
	}

	Dfs();

	printf("%d", Ans);

	return 0;
}
```