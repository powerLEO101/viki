# Luogu1967 货车运输

---

## 题面

[Luogu](https://www.luogu.org/problemnew/show/P1967)

---

## 题解

在很久之前就切过这道题，然而在知道了Kruskal重构树后发现，这题TMD水的一比

首先简化一下这题的题意就是要让两点之间的路径上的权值最小的边最大，然后求这条边的边权

然后。。。就没有然后了，Kruskal重构树直接切。。。

---

## 代码

```c++
/**************************
  * Writer : Leo101
  * Problem : LG P1967 货车运输
  * Tags : Ex_Kruskal, LCA
**************************/
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
#define File(s) freopen(#s".in", "r", stdin); freopen(#s".out", "w", stdout)
#define gi get_int()
#define for_edge(i, x) for (int i = Head[x]; i != -1; i = Edges[i].Next)
const int MaxN = 100000, MaxEdge = 60000;
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

class edge
{
public:
	int From, To, Value;
}edges[MaxEdge];
bool operator< (const edge& a, const edge& b)
{
	return a.Value > b.Value;
}
int e_num, Node_num;
void add_edge(int From, int To, int Value)
{
	edges[e_num++] = (edge){From, To, Value};
}

class Edge
{
public:
	int Next, To;
}Edges[MaxN << 1];
int Head[MaxN], E_num, Val[MaxN];
void Add_edge(int From, int To)
{
	Edges[E_num] = (Edge){Head[From], To};
	Head[From] = E_num++;
}

int Father[MaxN];
void Init() {for (int i = 0; i < MaxN; i++) Father[i] = i;}
int Get_father(int u) {return Father[u] == u ? u : Father[u] = Get_father(Father[u]);}
void Ex_Kruskal()
{
	Init();
	std::sort(edges, edges + e_num);
	for (int i = 0; i < e_num; i++) {
		int From = edges[i].From, To = edges[i].To, Value = edges[i].Value;
		int Fx = Get_father(From), Fy = Get_father(To);
		if (Fx == Fy) continue;
		int Index = Node_num++;
		Father[Fx] = Father[Fy] = Index;
		Val[Index] = Value;
		Add_edge(Index, Fx); Add_edge(Fx, Index);
		Add_edge(Index, Fy); Add_edge(Fy, Index);
	}
}

int Up[MaxN][21], Deep[MaxN];
bool Vis[MaxN];
void Dfs(int Now, int Pre = -1)
{
	Vis[Now] = true;
	for_edge(i, Now) {
		int To = Edges[i].To;
		if (To == Pre) continue;
		Up[To][0] = Now;
		Deep[To] = Deep[Now] + 1;
		Dfs(To, Now);
	}
}
void Move(int& u, int& v)
{
	if (Deep[u] < Deep[v]) std::swap(u, v);
	for (int i = 20; i >= 0; i--)
		if (Deep[Up[u][i]] >= Deep[v]) 
			u = Up[u][i];
}
int Query(int u, int v)
{
	Move(u, v);
	for (int i = 20; i >= 0; i--)
		if (Up[u][i] != Up[v][i])
			u = Up[u][i], v = Up[v][i];
	return u == v ? u : Up[u][0];
}

int main()
{
	File(code);

	memset(Head, -1, sizeof(Head));

	int n = gi, m = gi;
	Node_num = n;
	for (int i = 0; i < m; i++) {
		int From = gi - 1, To = gi - 1, Value = gi;
		add_edge(From, To, Value);
	}

	Ex_Kruskal();
	for (int i = 0; i < n; i++) {
		int Fx = Get_father(i);
		if (Vis[Fx] == false) {
			Up[Fx][0] = Fx;
			Dfs(Fx);
		}
	}
	for (int j = 1; j < 21; j++)
		for (int i = 0; i < Node_num; i++)
			Up[i][j] = Up[Up[i][j - 1]][j - 1];
	int Q = gi;
	while (Q--) {
		int From = gi - 1, To = gi - 1;
		int Fx = Get_father(From), Fy = Get_father(To);
		if (Fx != Fy) {
			printf("-1\n");
			continue;
		} 
		printf("%d\n", Val[Query(From, To)]);
	}

	return 0;
}
```