---
layout: post
subtitle: 《Graph》2小节： 邻接矩阵，邻接表
gh-repo: ittarvin/c_ff
gh-badge: [star, fork, follow]
tags: [C++,graph]
comments: false
cover-img: /assets/img/IMG_1573.JPG
thumbnail-img: /assets/img/图-邻接表-有向图.png
share-img: /assets/img/IMG_1482.JPG
---
与邻接矩阵能方便的对顶点进行随机访问不同，在邻接表中要判断任意两个顶点 $v_i$ 和 $v_j$ 直接是否相连，需要遍历第i个或第j个单链表。

# 图论

## 邻接矩阵
邻接矩阵就是使用矩阵来描述图中顶点之间的关系，在程序语言中很容易使用二维数组来实现。
设$G=(V,E)$ 是一个图，其中$V=\{v_0,v_1,....,v_{n-1}\}$ 那么G的邻接矩阵A定义如下的n阶方阵：

$$
A[i][j] = 
\begin{cases}
1 若(v_i,v_j) 或 <v_i,v_j> 是E中的边\\
0 若(v_i,v_j) 或 <v_i,v_j> 不是E中的边
\end{cases}
$$

[有向图](/2022-05-31-graph)和[无向图](/2022-05-31-graph)的邻接矩阵分别为M1和M2：

$$
M1
\begin{bmatrix} 
0&1&0&0\\
1&0&1&0\\
1&0&0&1\\
0&0&0&0\\
\end{bmatrix}

M2
\begin{bmatrix}
0&1&1&0\\
1&0&1&0\\
1&1&0&1\\
0&0&1&0\\
\end{bmatrix}
$$

利用邻接矩阵可以判断任意两点之间是否有边，并容易求各个顶点的度：
无向图:

$$
D(v_i) = \sum_{j=0}^{n-1} A[i][j] （或 \sum_{j=0}^{n-1}A[j][i]）
$$

有向图:

$$
OD(v_i) = \sum_{j=0}^{n-1} A[i][j] 
$$

$$
ID(v_i) = \sum_{j=0}^{n-1} A[i][j]
$$

用邻接矩阵表示带权图：

$$
A[i][j] = 
\begin{cases}
W_{ij} 若(v_i,v_j) 或 <v_i,v_j>  \in E\\
\infty ,否则。
\end{cases}
$$

##  邻接表
邻接表是顺序存储与链式存储相结合的方法。在邻接表中，对图中每个顶点建立一个单链表。
- 对无向图，第 i 个单链表中的结点表示依赖顶点$v_i$ 的边。
- 对有向图，是以顶点$v_i$为尾的弧，这个单链表称为顶点$v_i$的邻接表。
- 每个单链表设置一个表头结点，每个表头结点有两个域 vertex 和 firstarc，vertex 用来存储顶点$v_i$的信息，firstarc 指向邻接表中第一个结点。
- 为了便于随机访问，将所有单链表的表头组成一个一维数组 Adjlist 。
- 单链表中的每个结点称为表结点，包括两个域：邻接点域（adjvex），和链域（nextarc），邻接点域用来存放于 $v_i$ 相邻的顶点的数组中序号，链域用以指向同$v_i$邻接的下一个结点。
- 在带权的表结点中可以增加一个权值域，用于存储权值（weight）。

![图-邻接表-无向图.png](/assets/img/图-邻接表-无向图.png)

算法描述如下：

```cpp

#define vnum 20

typedef struct arcnode{
    int adjvex;
    struct arcnode * nextarc;
}ArcNode;

typedef struct vexnode{
    int vertex;
    ArcNode *firstarc;
}AdjList[vnum];

typedef struct gp{
    AdjList adjlist;
    int vexnum,arcnum;
}Graph;

```

对于有向图，有时需要建立一个逆邻接表，己对每一个顶点$v_i$建立一个以$v_i$为弧头的邻接点的链表，这同邻接表正好相反，对于逆邻接表可以很容易求出$v_i$的入度。

![图-邻接表-有向图.png](/assets/img/图-邻接表-有向图.png)
