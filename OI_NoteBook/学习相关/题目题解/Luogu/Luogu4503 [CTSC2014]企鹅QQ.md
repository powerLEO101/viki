# Luogu4503 [CTSC2014]企鹅QQ

---

## 题面

[题目链接](https://www.luogu.org/problemnew/show/P4503)

---

## 题解

因为只有一个字符不相同，所以我们可以考虑把每个串都再看有多少个相同的

通过Hash实现

因为这题卡Hash所以做人要有信仰

---

```c++
/**************************
  * Writer : Leo101
  * Problem : Luogu P4503 [CTSC2014]企鹅QQ
  * Tags : Hash
**************************/
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <string>
#define File(s) freopen(#s".in", "r", stdin); freopen(#s".out", "w", stdout)
#define gi get_int()
const int Max_N = 30003, Max_Len = 233;
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

int n, l, m;
unsigned long long Before[Max_N][Max_Len], Behind[Max_N][Max_Len], Val[Max_N];

void Hash(int Index)
{
	std :: string Str;
	std :: cin >> Str;
	Before[Index][0] = Str[0];
	for (int i = 1; i < l; i++) 
		Before[Index][i] = Before[Index][i - 1] * 131 + Str[i];
	for (int i = l - 1; i >= 0; i--) 
		Behind[Index][i] = Behind[Index][i + 1] * 137 + Str[i];
}

unsigned long long Get(int i, int j) { return j < 0 ? 0 : Before[i][j]; }

int main()
{
	File(code);

	n = gi, l = gi, m = gi;
	for (int i = 0; i < n; i++) Hash(i);

	int Answer = 0;
	for (int j = 0; j < l; j++) {
		for (int i = 0; i < n; i++) Val[i] = Get(i, j - 1) * 233 + Behind[i][j + 1] * 211;
		std :: sort(Val, Val + n);
		int Count = 1;
		for (int i = 0; i < n - 1; i++) {
			if (Val[i] == Val[i + 1]) Answer += Count, Count++;
			else Count = 1;
		}
	}

	printf("%d", Answer);

	return 0;
}
```