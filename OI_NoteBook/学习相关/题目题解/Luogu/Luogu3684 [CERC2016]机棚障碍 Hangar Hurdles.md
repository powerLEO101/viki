# Luogu3684 [CERC2016]机棚障碍 Hangar Hurdles

---

## 题面

[题目链接](https://www.luogu.org/problemnew/show/P3684)

---

## 题解

用Bfs将每个点所能放的最大的箱子长度都算出来，那么就得到了一个矩阵

那么我们要求的就是这个矩阵内两个点的一条路径，求这条路径的点权的最小值的最大值

可以用Kruskal重构树，边权就是点权的Min

---

## 代码

```c++
/**************************
 * Writer : Leo101
 * Problem : Luogu P3684 [CERC2016]机棚障碍 Hangar Hurdles
 * Tags : Kruskal重构树，二分
 **************************/
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <string>
#include <queue>
#define File(s) freopen(#s".in", "r", stdin); freopen(#s".out", "w", stdout)
#define gi get_int()
#define INF 0x3f3f3f3f
#define for_edge(i, x) for (int i = Head[x]; i != -1; i = Edges[i].Next)
const int Max_N = 1400;
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

int Value[Max_N][Max_N], Map[Max_N][Max_N];
int Gox[8] = {0, 0, 1, -1, 1, -1, 1, -1}, Goy[8] = {1, -1, 0, 0, 1, 1, -1, -1};

void Get_Dist(int n)
{
    std :: queue<int> Qx, Qy;
    memset(Value, 0x3f, sizeof(Value));

    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            if (Map[i][j] == 1) {
                Qx.push(i), Qy.push(j); 
                Value[i][j] = 0;
            }

    while (!Qx.empty()) {
        int x = Qx.front(), y = Qy.front();
        Qx.pop(); Qy.pop();
        for (int i = 0; i < 8; i++) {
            int Gx = x + Gox[i], Gy = y + Goy[i];
            if (Gx < 0 || Gy < 0 || Gx >= n || Gy >= n)
                continue;
            if (Value[Gx][Gy] != INF || Map[Gx][Gy] == 1) continue;
            Value[Gx][Gy] = Value[x][y] + 1;
            int Max_Val = std :: min(std :: min(Gx, Gy) + 1, std :: min(n - Gx, n - Gy));
            Value[Gx][Gy] = std :: min(Value[Gx][Gy], Max_Val);
            Qx.push(Gx); Qy.push(Gy);
        }
    }

    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            if (Value[i][j] != 0)
                Value[i][j] = 2 * (Value[i][j] - 1) + 1;
}

class edge
{
public:
    int From, To, Value;
} edges[Max_N * Max_N * 2];
int e_num;
void add_edge(int From, int To, int Value)
{  edges[e_num++] = (edge) {From, To, Value}; }
bool operator< (const edge& a, const edge& b)
{ return a.Value > b.Value; }

int Father[Max_N * Max_N * 2];
int Get_father(int u) 
{ return Father[u] == u ? u : Father[u] = Get_father(Father[u]); }

class Edge {
public:
    int Next, To;
} Edges[Max_N * Max_N * 2];
int Head[Max_N * Max_N * 2], E_num;
int Node[Max_N * Max_N * 2], N_num;
void Add_edge(int From, int To)
{
    Edges[E_num] = (Edge) {Head[From], To};
    Head[From] = E_num++;
}

void Build_Graph(int n)
{
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (Value[i][j] == 0) continue;
            for (int x = 0; x < 4; x++) {
                int Gx = i + Gox[x], Gy = j + Goy[x];
                if (Gx < 0 || Gy < 0 || Gx >= n || Gy >= n)
                    continue;
                if (Value[Gx][Gy] == 0) continue;
                add_edge(i * n + j, Gx * n + Gy, std :: min(Value[i][j], Value[Gx][Gy]));
            }
        }
    }

}
void Ex_Kruskal(int n)
{
    Build_Graph(n);
    std :: sort(edges, edges + e_num);
    for (int i = 0; i < Max_N * Max_N * 2; i++) Father[i] = i;

    for (int i = 0; i < e_num; i++) {
        int From = edges[i].From, To = edges[i].To, Value = edges[i].Value;
        int Fa_F = Get_father(From), Fa_T = Get_father(To);
        if (Fa_F == Fa_T) continue;
        Father[Fa_T] = N_num; Father[Fa_F] = N_num;
        Add_edge(N_num, Fa_F);
        Add_edge(N_num, Fa_T);
        Node[N_num++] = Value;
    }
}

int Up[Max_N * Max_N * 2][21], Depth[Max_N * Max_N * 2];
bool Vis[Max_N * Max_N * 2];
void Dfs(int Now, int Pre = -1)
{
    Vis[Now] = true;
    for_edge(i, Now) {
        int To = Edges[i].To;
        if (Vis[To] == true) continue;
        Up[To][0] = Now;
        Depth[To] = Depth[Now] + 1;
        Dfs(To, Now);
    }
}
void Move(int& u, int& v)
{
    if (Depth[u] < Depth[v]) std :: swap(u, v);
    for (int i = 20; i >= 0; i--)
        if (Depth[Up[u][i]] >= Depth[v])
            u = Up[u][i];
}
int LCA(int u, int v)
{
    Move(u, v);
    for (int i = 20; i >= 0; i--) 
        if (Up[u][i] != Up[v][i])
            u = Up[u][i], v = Up[v][i];
    if (Up[u][0] != Up[v][0]) return 0;
    return u == v ? u : Up[u][0];
}

int main()
{
    memset(Head, -1, sizeof(Head));

    int n = gi;
    N_num = n * n;
    for (int i = 0; i < n; i++) {
        std :: string Str;
        std :: cin >> Str;
        for (int j = 0; j < n; j++)
            Map[i][j] = Str[j] == '.' ? 0 : 1;
    }

    Get_Dist(n);
    Ex_Kruskal(n);

    for (int i = N_num - 1; i >= 0; i--) {
        if (Vis[i] == true) continue;
        Up[i][0] = i;
        Dfs(i);
    }

    for (int j = 1; j < 21; j++) 
        for (int i = 0; i < N_num; i++) 
            Up[i][j] = Up[Up[i][j - 1]][j - 1];

    int Q = gi;
    while (Q--) {
        int x1 = gi - 1, y1 = gi - 1, x2 = gi - 1, y2 = gi - 1;
        int From = x1 * n + y1, To = x2 * n + y2;
        printf("%d\n", Node[LCA(From, To)]);
    }

    return 0;
}

```