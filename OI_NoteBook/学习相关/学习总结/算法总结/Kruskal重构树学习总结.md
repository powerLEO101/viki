# Kruskal重构树学习总结

---

emmm，好东西，感觉可以现学现用，还让我毫无压力的切掉了NOIP2013的货车运输，高科技啊

---

## 正文

首先Kruskal重构树显然是可以对图做的。

Kruskal重构树有一下几个性质：

- 这是一个二叉堆
- 原图中两点路径上的的边权最大值（最小值）是重构树中两点LCA的点权
- 原图中的点在重构树中全都书叶子节点

Kruskal重构树的构造：

按照Kruskal的思路，先排序，使用并查集辅助加边

对于一条可以加的边：

1. 新建节点Index（编号从n开始）
2. 将两点原有集合改为Index
3. 将原有集合的Root分别与Index连边
4. Index的点权为这两条边的边权

然后需要注意的是：Ex_Kruskal是从小到大排序那么两点的LCA的权值为路径上的最大值，从大到小排序才是最小值

---

## 参考

https://blog.csdn.net/hwzzyr/article/details/81190442