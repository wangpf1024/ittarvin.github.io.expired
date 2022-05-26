---
layout: post
subtitle: 《查找》2小节： 二叉排序树（Binary Sort Tree）
gh-repo: ittarvin/c_ff
gh-badge: [star, fork, follow]
tags: [C++,search]
comments: false
cover-img: /assets/img/IMG_1414.JPG
thumbnail-img: /assets/img/二叉排序树.png
share-img: /assets/img/IMG_1482.JPG
---
静态查找表一旦生成之后，所含数据元素（在检索阶段）是固定不变的。二叉排序树是一种实现动态查找的树表，这种树表的结构本身是在查找过程中动态生成的，即对于给定的 key 值，若表中存在与 key 值相等元素则查找成功，不然 ，插入关键字不等于 key 的元素。

# 查找

## 二叉排序树
一棵二叉排序树（Binary Sort Tree）又称二叉查找树，或者一颗空二叉树，或者是具有一下性质的二叉树：
1. 若它的左子树不为空，则左子树所有结点的值均小于它的根结点的值。
2. 若它的右子树不为空，则右子树所有结点的值均大于它的根结点的值。
3. 根的左，右子树也分别为二叉排序树。 
如图所示：
![二叉排序树.png](/assets/img/二叉排序树.png)

###  二叉排序树的查找
由于二叉排序树的特点，要找到键值比某个键值小（大）的结点，只需要通过次结点的左指针（右）到它的左（右）子树中去找。查找 key = 76，key = 22 的过程如上图所示。

算法描述如下：

```cpp
typedef   struct   bstnode{

  int data;

  struct   bstnode *lchild,*rchild;

}*BSTree;

static BSTree searchBST(BSTree bst,int key){

  if(bst == NULL) return NULL;

  if(key == bst ->data) return bst;

  else if (key < bst-> data) return searchBST(bst ->lchild, key);

  else return searchBST(bst ->rchild, key);

};
```

与[二分查找](/2022-05-25-search)类似，关键字的比较次数不超过二叉树的深度。对同一组结点，由于建立二叉树的插入结点的先后次序不同，所构成的二叉排序树的形态和深度也不同，含有 n 个结点的二叉树不是唯一的。也就是说，二叉排序树上的查找长度不仅与树的结点 n 有关，也与二叉排序生成的过程有关。

###  二叉排序树的插入

由于二叉排序树这种动态树表是在查找过程中，不断的往树中插入不存在的键值而形成的，所以插入算法必须包括查找过程，并且在查找不成功的情况下进行插入新结点的操作。在二叉排序树上进行插入的原则：必须要保证插入一个新的结点后，仍然是一颗二叉排序树。这个结点是查找不成功时，查找路径上的最后一个结点的左孩子或右孩子。如图所示 查找 key = 22 不成功时，到达结点 20 的右子树，该位置恰好是新结点 22 的插入位置。

![二叉排序树-插入.png](/assets/img/二叉排序树-插入.png)
算法描述如下：

```cpp
typedef   struct   bstnode{

  int data;

  struct   bstnode *lchild,*rchild;

}BSTNode,*BSTree;

  

static BSTree bst;

  

static   BSTNode *f;

 
static BSTree searchBST2(BSTree bst,int key){

  if(bst == NULL) {

  return NULL;

  }

  f = bst;

  if(key == bst -> data) {

  return bst;

  }else if (key < bst -> data){

  return searchBST2(bst ->lchild, key);

  }else {

  return searchBST2(bst ->rchild, key);

  }

};

static   int   insertBST(int key){

  BSTNode *p,*t;

  t = searchBST2(bst,key);

  if(t == NULL){

  p = (bstnode *)malloc(sizeof(bstnode));

  p ->data = key;

  p ->lchild = NULL;

  p ->rchild = NULL;

  if(f == NULL){

  bst = p;

  }else if(key < f ->data){

  f -> lchild = p;

  }else{

  f -> rchild = p;

  }

  return 1;

  }

  return 0;

}
```

这里，应该注意的是，每次插入新的结点都是二叉排序树上新的叶子结点，即进行插入操作时，不必移动其他结点，仅需要改动某个结点的指针，由空变为非空就行。这里也就说明了，输入键值序列的键值排列顺序不同，构造的二叉排序树的结构也会不同。

###  二叉排序树的分析

二叉排序树的平均查找长度介于 $O(n)$ 和 $O(log_2n)$ 之间，查找效率与树的形态有关。如下图（a）的平均查找长度为$O(log_2n)$ ，图中（b）的二叉排序树退化为一条单支，查找算法退化为顺序查找，平均查找长度上升为 $(n+1)/2$，及平均查找长度是 $O(n)$ 。为了避免出现单支的情况需要在二叉排序树的动态变化过程中随时调整其形态，使之保持“平衡”。

![二叉排序树-退化.png](/assets/img/二叉排序树-退化.png)
