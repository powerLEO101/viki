# Luogu3293 [SCOI2016]美味

---

## 题面

[题目链接](https://www.luogu.org/problemnew/show/P3293)

---

## 题解

首先这题如果没有偏好值那么就是一个傻逼贪心题（Trie）

然而有了偏好值后，我们就不能直接根据Trie来贪心了

因为现在所有的数都是动态的了

借鉴Trie树贪心的思路，从高位往地位贪心

若现在贪心到第$i$位，$b$的第i位为$1$，前面已经贪心到的最优的与b异或的值为Ans

那么我们就要找在$[Ans+00000, Ans+01111]$这个区间内有没有数

可以用主席树来完成这个操作

---

## 代码

```c++
/**************************
  * Writer : Leo101
  * Problem : Luogu P3293 [SCOI2016]美味
  * Tags : 主席树，贪心
**************************/
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
#define File(s) freopen(#s".in", "r", stdin); freopen(#s".out", "w", stdout)
#define gi get_int()
const int Max_N = 5e5, Max_Ai = 2e5;
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

class Node
{
public:
	int Val;
	Node* l_son,* r_son;
Node() {
	Val = 0;
	l_son = r_son = NULL;
}
}* Root[Max_N];
int Num[Max_N];
int Get(Node* Root) { return Root == NULL ? 0 : Root -> Val; }
Node* Get(int Index) { return Index < 0 ? NULL : Root[Index]; }
Node* l_son(Node* Root) { return Root == NULL ? NULL : Root -> l_son; }
Node* r_son(Node* Root) { return Root == NULL ? NULL : Root -> r_son; }
Node* Modify(Node* Root, int Index, int l = 0, int r = Max_Ai)
{
	Node* New = new Node;
	if (l == r - 1) {
		New -> Val = Get(Root) + 1;
		return New;
	}

	int Mid = (l + r) / 2;
	New -> l_son = l_son(Root); New -> r_son = r_son(Root);
	if (Index < Mid) New -> l_son = Modify(New -> l_son, Index, l, Mid);
	else New -> r_son = Modify(New -> r_son, Index, Mid, r);
	New -> Val = Get(New -> l_son) + Get(New -> r_son);

	return New;
}
int Query(Node* Root1, Node* Root2, int Vl, int Vr, int l = 0, int r = Max_Ai)
{
	if (Vl <= l && r <= Vr) return Get(Root2) - Get(Root1);
	if (l == r - 1) return 0;

	int Mid = (l + r) / 2, Ret = 0;
	if (Vl < Mid) Ret += Query(l_son(Root1), l_son(Root2), Vl, Vr, l, Mid);
	if (Mid <= Vr) Ret += Query(r_son(Root1), r_son(Root2), Vl, Vr, Mid, r);

	return Ret;
}
bool Query(int l, int r, int Vl, int Vr)
{ return Query(Get(l - 1), Get(r), Vl, Vr + 1) >= 1; }

int Get_ans(int b, int x, int l, int r)
{
	int Ans = 0;
	for (int i = 20; i >= 0; i--) {
		int Now = Ans + ((((b >> i) & 1) ^ 1) << i);
		if (Query(l, r, Now - x, Now + (1 << i) - 1 - x) == true) Ans = Now;
		else Ans += ((b >> i) & 1) << i;
	}

	return Ans ^ b;
}

int main()
{
	File(code);

	int n = gi, m = gi;
	for (int i = 0; i < n; i++) Num[i] = gi;
	
	for (int i = 0; i < n; i++) {
		Root[i] = Get(i - 1);
		Root[i] = Modify(Root[i], Num[i]);
	}

	while (m--) {
		int b = gi, x = gi, l = gi - 1, r = gi - 1;
		printf("%d\n", Get_ans(b, x, l, r));
	}

	return 0;
}
```