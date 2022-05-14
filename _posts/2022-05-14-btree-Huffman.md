---
layout: post
subtitle: 《树与二叉树》5小节：哈夫曼（Huffman）树
gh-repo: ittarvin/c_ff
gh-badge: [star, fork, follow]
tags: [C++,tree，huffman]
comments: false
cover-img: /assets/img/IMG_1615.JPG
thumbnail-img: /assets/img/哈夫曼树的构建方法.png
share-img: /assets/img/IMG_1482.JPG
---
文中梳理了丛分类树到Huffman树的相关理论与代码实践。

# 哈夫曼（Huffman）树与哈夫曼算法
## 分类与判定树
树有广泛的应用，其中主要的应用是描述分类过程。分类是一种常用的运算，其作用是将输入的数据按照预定的标准划分成不同的种类。例如，将某个地区的人口按它们的年龄age确定其列表就是一个分类问题，算法描述如下：

```cpp
 char classfy(int age){

  if(age < 18)return 'A';

  else if (age < 45) return 'B';

  else if (age < 60) return 'C';

  else   return   'D';

}
```
上述算法描述被频繁的重复使用，这个时候就要考虑其时间性能。显而易见的是分类算法主要工作是条件判断，因此将条件判断作为标准操作是合理的。又由于分类算法的每一个条件判断对应于相应判断树上的一个非终端结点，因而可以在判断树上讨论对应的分类算法的时间复杂度。以下给出两种不同判断树的示例图，并比较两者的评价比较次数。
![判断树示意图.png](/assets/img/判断树示意图.png)

假设该地区的有人口 N = 10000 人，并且各个类别的人数分布如下表所示，对于每一类别的人，类别人数乘以为区分该分类需要比较的数就等于该类别所有人确定类别所比较的次数。

| 类别  | A        | B                  | C                  | D             |
|-----|----------|--------------------|--------------------|---------------|
| 年龄  | age < 18 | 18 $leq$ age < 45 | 45 $leq$ age < 60 | age $geq$ 60 |
| 百分比 | 0.2      | 0.3                | 0.25               | 0.25          |

图 a 的比较平均比较次数：

SUM =  $N\times0.2 times1+N\times0.3\times2+N\times0.25\times3+N\times0.25times3$

= $N\times(0.2+0.6+0.75+0.75)$

= $10000\times2.3$

= $23000$

平均比较次数为：$SUM\div N = 2.3$

图 b 的比较平均比较次数：

SUM =  $N\times0.2 times2+N\times0.3\times2+N\times0.25\times2+N\times0.25\times2$

= $N\times(0.4+0.6+0.5+0.5)$

= $10000\times2$

= $20000$

平均比较次数为：$SUM\div N = 2$


图 b 相对应图 a 的比较次数少了 3000 次比较，平均比较次数也下降为 2。这就说明，按照不同的判断树进行分类的计算量是不同的，有时候差距很大。是否存在计算量最小的判定树呢？

## 哈夫曼树与哈夫曼算法
为解决上述问题，首先必须找到一种一般性方法以确认一颗判断树的评价比较次数。

设 T 是一颗判断树，其终端结点为 $n_1,...n_k$。每个终端结点$n_i$对应的百分比$p_i$(即输入数据落入$n_i$ 的概率为 $p_i$)，这里

$$
sum_{i=1}^k p_i = 1
$$

通常将 $p_i$ 称为 $n_i$ 的权。再假定丛根结点到 $n_i$ 的结点树为 $1_i$ ,显然，为了区分出 $n_i$ 对应的分类结果就需要做 $1_i$ 次比较。例如 图b中的叶子B有两个祖先，它们正好是为了区分B进行了两次比较。按判断树T进行分类的平均比较次数记为 WPL(T),则

$$
WPL(T) = (sum_{i=1}^k (p_i\times N\times 1_i))/N
$$

简化为：

$$
WPL(T) = sum_{i=1}^k (p_i\times1_i)
$$

现在，问题可以重新表述为：给一组值$p_1,...p_k$，如何构建一颗有k个叶子且分别以这些值为权的判断树，使得其平局比较次数最小。满足上述条件的判断树称为哈夫曼树。哈夫曼率先给出了一个简单有效的方法，称为哈夫曼算法，描述如下：

1. 由给定的值 {$p_1,...,p_k$} 构造森林 F = {$T_1,...,T_k$} ，其中每个 $T_i$ 为一颗只有根节点且其权重为 $p_i$ 的二叉树。

2. 丛 F 中选取根节点的权最小的两颗树$T_i$ 和 $T_j$,构造一颗分别以 $T_i$ 和 $T_j$ 为左，右子树的二叉树$T_h$,置 $T_h$ 根节点权值为 $T_i$ 和 $T_j$ 根节点的权值之和。

3. 丛F中删除$T_i$ 和 $T_j$，并将$T_h$加入F。若F中仍多于一颗二叉树，则返回2，直到F中只含有一颗二叉树为止，这颗二叉树就是哈夫曼树。

图b的构建过程如下：
![哈夫曼树的构建方法.png](/assets/img/哈夫曼树的构建方法.png)
算法描述如下：
```cpp
typedef   struct   huffmanNode{

  float w;

  int   parent,lchild,rchild;

}huffmanNode;

  

typedef   huffmanNode   hftree[2*n-1];

/

  k : 表示构造哈夫曼树之前给定的权值个数。

  w：权值

  t： 哈夫曼树

 */

void huffman(int k,float w[],hftree t){

  int i,j,x,y;

  float m,n;

  //置初态：读取 k 个权值到数组 t 的前 k 个分量中。

  for(i=0;i<2*k-1;i++){

  t[i].parent = -1;

  t[i].lchild = -1;

  t[i].rchild = -1;

  if(i < k){

  t[i].w = w[i];

  }else{

  t[i].w = 0;

  }

  }

  //构建二叉树

  for(i=0;i<k-1;i++){

  x = 0;y =0;

  m = 9999;n=9999;

  //找两颗权值最小的二叉树

  for(j=0;j<k+i;j++){

  if((t[j].w < m) && (t[j].parent) ==-1){

  n = m;

  y = x;

  m = t[j].w;

  x = j;

  }else if((t[j].w < n) && (t[j].parent) ==-1){

  n = t[j].w;

  y = j;

  }

  }

  //合并新的二叉树

  t[x].parent = k + i;

  t[y].parent = k + i;

  t[k + i].w = m + n;

  t[k + i].lchild = x;

  t[k + i].rchild = y;

  }

}
```

## 哈夫曼编码
在通信领域，可以应用哈夫曼树来设计字符传输的编码。通常希望字符在传输过程中总的编码长度越短越好。考虑到一个代传输到文本中字符出现的评论不同，直观的想法是让出现频率较多的字符采用较短的编码，则传输的字符总长度就会减少。
假设一个代传输的文本中有 **n** 个不同的字符，每种字符出现的频率为 $w_i$ 该字符的编码长度为 $m_i$ 。则总码长为：

$$
\sum_{i=1}^n w_i m_i
$$

设总的字符个数为S，则：平均编码长度

$$
\frac{1}{S}\sum_{i=1}^n w_i m_i = \sum_{i=1}^n\frac{w_i}{S}m_i = \sum_{i=1}^n p_i m_i
$$

其中$p_i$是第i种字符出现频率。可以看出当平均码长达到最短时，文本总的码长也到最短。
将哈夫曼树的每一个左分支标记为 “0”，每一个右分支标记为 “1”，这样丛根到每个叶子结点形成 “0”/“1” 序列，将该序列作为叶子结点对应的字符编码，由次得到一个二进制编码称为哈夫曼编码。如图所示：
![哈夫曼编码.png](/assets/img/哈夫曼编码.png)