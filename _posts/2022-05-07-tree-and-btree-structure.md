---
layout: post
subtitle: 《树与二叉树》1小节：基本概念
gh-repo: ittarvin/c_ff
gh-badge: [star, fork, follow]
tags: [数据结构]
comments: true
cover-img: /assets/img/IMG_1615.JPG
thumbnail-img: /assets/img/数据结构导论.jpeg
share-img: /assets/img/IMG_1482.JPG
---

文中梳理了有关树，树的相关术语，二叉树的性质及二叉树的存储结构与代码实践。

# 树的基本概念
## 树的基本概念
树（Tree）是一种重要的数据结构，其定义如下：
树是 n ( $n \geq 0$) 个节点的有限集合，一颗树满足以下两个条件：

1. 当 n = 0 时，成为 空树。

2. 当 n > 0 时，有且仅有一个根节点，除根节点外，其余节点 m （$m\geq0$）个互不相交的非空结合 T1,T2,T3......Tm，这些集合中的每一个都是一颗树，成为子树。
   森林（Forest）是 m（$m\geq0$）颗互不相交的树的集合。树的每个结点的子树是森林。删除一个非空的根结点，它的子树便构成了森林。

## 树的相关术语
内容描述

1. 结点的度：树上任一结点所拥有的的子树的数目称为该结点的度。

2. 叶子：度为 0 的结点称为叶子或终端结点。

3. 树的度：一颗树中所有结点的度的最大值称为该树的度。一个结点的子树的根称为该结点的孩子（或子结点），相应的该结点称为孩子的双亲（父结点）。

4. 结点的层次：从根开始算起，根的层次为 1，其余层次在双亲结点的层次上加 1。

5. 树的高度：一颗树中所有结点层次的最大值称为该树的高度或深度。

6. 有序树：若树中各结点的子树从左到右是有次序的，不能交换，称为有序树。

7. 无序树：若树中各结点的子树是无序的，可以交换，则称为无序树。

# 二叉树

二叉树（Binary Tree）是 n  $(n\geq0)$个元素的有限集合，该集合或为空，或者由**一个根及两颗互不相交的**左子树和右子树组成，其中左子树和右子树也均为二叉树。
二叉树的任何一个结点都有两颗子树（它们中任何一个都可以为空树），并且这两颗子树是有次序关系，即如果互换了位置就称为了一颗不同的二叉树。

## 二叉树的性质
内容描述

1. 二叉树第i  $(i\geq0)$ 层上至多有 $2^{i-1}$个结点。

2. 深度为k $(k\geq0)$的二叉树至多有 $2^{k}-1$ 个结点。

3. 对任何一颗二叉树，若度为0的结点个树为n0，度为2的结点个树n2，则 n0 = n2 + 1；
   **满二叉树**:深度为k且有$2^{k}-1$ 个结点的二叉树称为满二叉树。
   **完全二叉树**:如果对**满二叉树**按从上到下，从**左到右**顺序编号，并在最下一层删去部分结点（最后一层仍有结点），如果删除的这些结点的编号是连续的且删除的结点中包含有最大的结点，那么这颗二叉树就是完全二叉树。

4. 含有n个结点的完全二叉树的深度为 $\lfloor log_2 n \rfloor$ + 1 。

5. 如果一颗有n个结点的二叉树**按层编号**。则对任一编号i（$1\leq i\leq n$）的结点A有

- 若 i = 1 ,则结点A是根；若 i > 1 ,则 A的双亲 Parent（A）的编号为  $\lfloor i/2 \rfloor$。

- 若 $2\times i> n$ ，则结点 A 既无左孩子，也无右孩子；否则 A的左孩子Lchild（X）的编号为 $2\times i$。

- 若 $2\times i + 1 >n$, 则结点A无右孩子；否则，A的右孩子Rchild（A）的编号为 $2\times i + 1$。

# 二叉树的存储结构
二叉树的存储结构包括：顺序存储结构，链式存储结构。

### 顺序存储结构

由二叉树的**性质5**可知，如果对一个任何一个完全二叉树的所有结点按层编号，则结点编号直接的关系可以准确的反映结点直接的逻辑关系，因此对于任何一个完全二叉树来说，可以讲编号作为一维数组下标的方式将结点存入一维数组中。也就是讲编号为 i 的节点存入一维数组的以 i 为下标的数组元素中。如下图：
![二叉树的顺序存储.png](../assets/img/二叉树的顺序存储.png)

例如：若在上图中查询节点 C 的左孩子 D，则根据二叉树的性质 5 ，从 C 的下标值 4 可以算出其左孩子的下标值 8。
$2\times4=8$ ,  $8<10$ ，所以C节点存在左孩子。假如查找 F节点右孩子 $2\times5+1=11$ ,  $11>10$ 所以 F结点没有右孩子。

如果需要顺序存储的非完全二叉树，首先必须使用某种方法将其转化为完全二叉树，为此可增设许多虚拟结点。如下图
![非完全二叉树的顺序存储.png](../assets/img/非完全二叉树的顺序存储.png)
**这种方式明显的缺点就是空间上的浪费。**

### 链式存储结构
链式存储结构分为：二叉，三叉链表

#### 二，三叉链表
在二叉链表中，data 域用于存储二叉树中结点中的数据信息；lchild 是指向左孩子的指针，rchild 是指向右孩子的指针。此外，每个二叉链表还必须有一个指向根的指针，成为根指针。在三叉链表中每个结点增加一个指针parent，指向该结点的双亲。如图所示：
![二三叉链表.png](../assets/img/二三叉链表.png)
若二叉树为空，怎 Root == NULL。若某个结点的孩子不存在，则相应的结点指针为 NULL。具有n个结点的二叉树中，有2n个指针域，其中只有 n -1 个指针指向左右孩子，其余 n  +1个指针域为 NULL。
类型定义如下：
```cpp
#include "BtreeNode.hpp"

//二叉树链表
typedef   struct   btnode{

  int data;

  struct   btnode *lchild,*rchild;

}*BinTree;
//三叉链表
typedef   struct   ttnode{

  int data;

  struct   ttnode *lchild,*parent,*rchild;

}*TBinTree;
```
二叉树的链式存储结构操作方便，结点直接的父子关系在二叉链表，三叉链表中被直接表达成对应存储结点之间的指针，而称为二叉树最常用的存储结构。