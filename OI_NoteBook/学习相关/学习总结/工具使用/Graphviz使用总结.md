# Graphviz使用总结

---

**这东西真是贼JB好用**

---

## 安装

### Windows安装
[官网](http://www.graphviz.org/)

选Stable 2.38 Windows install packages

![Windows](_v_images/windows_1540371033_2920.png)

选择Zip格式

解压后需要添加Path路径（以Win10为例）

```dot
digraph {
        rankdir=LR
        node[shape=box]
        此电脑->属性->高级系统设置->环境变量->双击Path->新建->将Grpahviz解压后的bin目录路径添加到里面
}
```

![Add_To_Path](_v_images/add_to_pat_1540371402_23516.png)

---

### Ubuntu安装

Ubuntu可以直接用apt-get安装：

```
sudo apt-get install graphviz
```

---

### 测试是否安装成功

在终端或cmd中输入

```
dot -version
```

显示如下结果表示安装成功（卡在那里没关系，强制退出即可）

![测试](_v_images/测试_1540371536_18447.png)

---

## 使用

### 最简单的画图

有向图：

```dot
digraph G {
        a -> b;
        b -> c;
        c -> a;
}
```

无向图：

```dot
graph G {
        a -- b;
        b -- c;
        c -- a;
}
```

其中G为图的名称，可以随便取

### 稍微NB一点的小操作（以无向图为例，有向图同理）

一次添加多个边：

```dot
graph G {
        a -- {b,c,d,e}
}
```

修改边和点的颜色：

```dot
graph G {
        a[color=red]
        a -- b[color=red];
        b -- c[color=blue];
        c -- a[color=grey];
}
```

修改点的排列顺序：

原来

```dot
graph G {
        a -- b;
        b -- c;
        c -- d;
}
```

现在

```dot
graph G {
        rankdir=LR
        a -- b;
        b -- c;
        c -- d;
}
```

添加边的名字（可以当边权来用）

```dot
graph G {
        a -- b;
        b -- c[label="3"];
        c -- a[label="4"];
}
```

对所有边执行操作

```dot
graph G {
        edge[label="233", color=blue]
        a -- b;
        b -- c;
        c -- a;
}
```

修改点的形状

```dot
graph G {
        a[shape=box]
        a -- b;
        b -- c;
        c -- a
}
```

对所有点执行操作：

```dot
graph G {
        node[color=blue, shape=triangle]
        a -- b;
        b -- c;
        c -- a;
}
```

批量操作

```dot
graph G {
        a,b,c,d[color=blue]
        a -- b;
        b -- c;
        c -- d;
        d -- e;
        e -- a;
}
```

修改出边的方向

支持`w, e, s, n, sw, se, nw, ne`

```dot
graph G {
        a:e -- b:s;
        b:nw -- c:se;
        c -- a;
}
```

修改对点的描述（感觉没用）

```dot
graph G {
        a[label="233"]
        a -- b;
        b -- c;
        c -- a;
}
```

一个类似结构体的东西

需要将点的形状改为`record`，所以只能默认方形

```dot
graph G {
        node[shape=record];
        a[label="{a|233}"];
        b[label="{b|}"];
        a -- b;
        b -- c;
        c -- d;
}
```