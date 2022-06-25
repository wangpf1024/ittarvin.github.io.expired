---
layout: post
subtitle: 《排序》2小节： 快速排序，堆排序，二路归并排序
gh-repo: ittarvin/c_ff
gh-badge: [star, fork, follow]
tags: [数据结构]
comments: false
cover-img: /assets/img/IMG_1414.JPG
thumbnail-img: /assets/img/快速排序.png
share-img: /assets/img/IMG_1482.JPG
---
不同的目的，对边际成本的看法是不一样的。因此，在现实工作中，没有绝对的最好，只有在给定条件下相对比较好。

## 快速排序
快速排序是一种交换排序，实际上它是对冒泡排序的一种改进。它的基本思想：在n个记录中取一个记录键值为标准，通过一趟排序将待排序的记录分为小于或等于这个键值和大于这个键值的两个独立的部分，这时一部分键值均比另一部分键值小，然后，对这两部分继续分别进行快速排序，以达到整个序列有序。排序过程（第一次排序）如图所示：
![快速排序.png](/assets/img/快速排序.png)

算法描述如下：

```cpp
static int quickPartition(unSortedList3 R,int low,int high){

  int x = R[low].key;

  while (low < high) {

  while ((low<high) && R[high].key >= x) {

  high --;

  }

  R[low] = R[high];

  while ((low<high)&&R[low].key <= x) {

  low ++;

  }

  R[high] = R[low];

  }

  R[low].key = x;

  return low;

}

  

static void quickSort(unSortedList3 R,int low,int high){

  if(low < high){

  int idx = quickPartition(R, low, high);

  quickSort(R,low,idx-1);

  quickSort(R,idx+1,high);

  }

}
```
快速排序是通常情况下最好的算法, 其时间复杂度为 $O(nlog_2n)$，但是，在极端的情况下，它的复杂度是 $O(n^2)$，和[冒泡排序](../2022-05-14-sort)一样糟糕。而归并排序，即使在最坏的情况下，也能保证N乘以log（N）的复杂度，当然工程师们会想一些方法防止这种糟糕情况的发生。从这个例子可以看出，世界上没有绝对的好，常常有一得便有一失。

# 堆排序
##  堆定义
对[直接选择排序](../2022-05-14-sort)分析可知，在 $n$ 个记录中选出最小的值，至少要进行 $n-1$ 次比较。然而继续将剩余的 $n-1$ 个键值选出小值是否需要进行 $n-2$ 次比较呢？如何能利用前  $n-1$ 次比较所得信息，是否可以减少以后各自选择中的比较次数呢？基于上述分析，讨论一下堆排序。
堆定义如下：若有一个关键字序列 ${k_1,k_2,...k_n}$ 满足
$$
k_i \leq k_{2i}
$$
$$
k_i \leq k_{2i+1}
$$
或
$$
k_i \geq k_{2i}
$$
$$
k_i \geq k_{2i+1}
$$
其中，$i=1,2,..., \lfloor \frac{n}{2} \rfloor$ ,则称这个 $n$ 个键值序列${k_1,k_2,...k_n}$ 为最大堆活最小堆。这个序列的结构与[二叉树](../2022-05-07-tree-and-btree-structure)产生了完美对应关系。

## 堆排序
若在输出堆顶的最小（大）键值之后，使得剩余的 $n-1$ 个键值重新建成一个堆，则可得到 $n$ 个键值中的次最小（大）值。如此反复的过程，便能得到一个有序序列，这个过程称为堆排序。
因此，实现堆排序需要解决两个问题：

1. 如何由一个初始序列建成一个堆？

2. 如何在输出堆顶元素之后调整剩余元素称为一个新堆？

### 筛选
为解决堆排序中如何在输出堆顶元素之后调整剩余元素称为一个新堆的问题，假设现在已经存在一个最小堆如下图所示。在输出堆顶元素 **13** 之后，以最后一个元素 **92** 代替，此时只有堆顶元素改变了，其左，右子树均还是为堆的，所以只需要自上而下进行调整，得到一个新堆。做法步骤如下：
![堆排序-堆调整.png](/assets/img/堆排序-堆调整.png)

1. 首先以新的根节点 **92** 与其左，右子树的根节点的比较，选出三者最小作为根。
2. 由于右子树根结点 **27** 小于左子树根节点 **34** 且小于根节点 **92** ，因此将 **27** 与 **92** 交换。
3. 由于 **92** 代替了 **27** 之后破坏了右子树的堆结构，则需要进行步骤 1 和 2 相同的调整。

算法描述如下：

```cpp
static   void   siftR(unSortedList6 R,int k,int m){

  int i,j,x;

  i = k;

  j = 2 * i;

  x = R[k].key;

  recodeType t = R[k];

  while (j <= m) {

  if((j < m) && (R[j].key > R[j+1].key)){

  j++;

  }

  if(x < R[j].key)break;

  else{

  R[i] = R[j];

  i = j;

  j = 2 * i;

  }

  }

  R[i] = t;

}

  

static   void   swapR(unSortedList6 R,int min,int i){

  recodeType temp = R[min];

  R[min] = R[i];

  R[i] = temp;

}
```

### 排序
将一个初始化序列建成一个堆就是反复筛选的过程。其主要过程是：

1. 将要排序的所有键值看成一个完全二叉树的各个结点（不具备堆的性质）。
2. 根据完全二叉树性质：最后一个非终端节点是 $\lfloor \frac{n}{2} \rfloor$ 个元素，即对于 $i >\lfloor \frac{n}{2} \rfloor$ 的节点 $k_i$ 都没有孩子结点，因此以这样 $k_i$ 为更的子树已经是堆。
3. 筛选只需要从$\lfloor \frac{n}{2} \rfloor$ 开始，逐步把 $k_{\lfloor n/2 \rfloor},k_{\lfloor n/2 \rfloor-1},k_{\lfloor n/2 \rfloor-2},...,k_1$ 为根的子树 “筛选” 成堆，就完成了建堆过程。 

算法描述如下：

```cpp
static   void   heapSortR(unSortedList6 R,int n){

  int i;

  for (i = (n/2); i >= 1; i--) {

  siftR(R, i, n);

  }

  std::cout << "初始化堆 \\n";

  for (int j=1; j < 9; j++) {

  std::cout <<R[j].key<<" ";

  }

  std::cout <<"\\n";

  for (i = n; i>=2; i--) {

  swapR(R,1,i);

  siftR(R,1,i-1);

  }

}
```
堆排序在待排序记录较少时不适用，但对记录很多时候很有效，因为主要耗时运行时间是在建初始堆和不断筛选的过程。堆排序的算法时间复杂度为 $O(nlog_2n)$对于快速排序来说这是堆排序的最大优点。

## 二路归并排序
归并排序是于插入排序，交换排序，选择排序不同的一类排序方法，其不同之处在于要求待排序序列是由若干有序子序列组成。归并的意思是将两个或两个以上的有序序列合并成一个新有序表。合并方法是比较各个子序列的第一个记录的值，最小的一个就排序后序列的第一个记录值，取出这个记录，继续比较各个子序列现有的第一个记录值，便可找出排序后的第二个记录。如此继续下去，最终可以得到排序结果。因此归并的排序的基础是合并。假设有两个有序序列$$a_h,...,a_m$$

$$a_{m+1},...,a_n$$

它们相应的键值分别满足：

$$K_h \leq … \leq K_m$$

$$K_{m+1} \leq ... K_n$$

合并成一个有序序列$R_h,...R_n$, 使合并后的序列建站满足条件

$$K'_h \leq ...\leq K'_m \leq K'_{m+1} \leq ...\leq K'_n$$



算法描述如下：

```cpp
static   void   merge(unSortedList5 a, unSortedList5 R,int h,int m,int n){

  int k = h;

  int j = m;

  while ((h<m) && (j<n)) {

  if(a[h].key <= a[j].key){

  R[k] = a[h];

  h++;

  } else {

  R[k] = a[j];

  j++;

  }

  k++;

  }

  while (h<m) {

  R[k] = a[h];

  h++;

  k++;

  }

  while (j<n) {

  R[k] = a[j];

  j++;

  k++;

  }

  

  for (int i=0; i< n; i++) {

  std::cout <<a[i].key<<" ";

  }

  std::cout <<" --> ";

  for (int i=0; i< n; i++) {

  std::cout <<R[i].key<<" ";

  }

}
```

二路归并排序即是将量有序表合成一个有序表的排序方法，其基本思想：建设序列中有$n$个记录，可以看出 $n$ 个有序的子序列，每个序列的长度为 1。 首先将每相邻的两个序列合并，得到 $\lceil n/2 \rceil$ 个较大的有序子序列，每个子序列包括2个记录，再将上述子序列合并，得到 $\lceil \lceil n/2 \rceil /2 \rceil$ 个有序子序列，如此反复，知道得到一个长度为 n 的有序序列为止。排序过程如图所示：

![二路归并排序.png](/assets/img/二路归并排序.png)

算法描述如下：

```cpp
static   void   mergePass(unSortedList5 a,unSortedList5 b,int n,int h){

  int i=1;

  std::cout << "合并前a数组内容\n";

  for (int i=0; i<n; i++) {

  std::cout <<a[i].key<<" ";

  }

  std::cout << "\n";

  while (i<=n-2*h+1) {

  std::cout << "合并过程:";

  merge(a, b, i-1, i+h-1,i+2*h-1);

  std::cout << "\n";

  i += 2*h; 

  }

  

  if(i+h-1<n){

  merge(a, b,i-1, i+h-1, n);

  }else{

  for (int t=i-1; t<=n; t++) {

  b[t] = a[t];

  }

  }

}

  

static   void   mergeSort(unSortedList5 a,int n){

  int m = 1;

  unSortedList5 b;

  while (m < n) {

  std::cout << "------------------------------合并元素长度（"<<m<<"）\n";

  mergePass(a, b, n, m);

  m = 2 * m;

  std::cout << "\n";

  std::cout << "------------------------------合并元素长度（"<<m<<"）\n";

  mergePass(b, a, n, m);

  m = 2 * m;

  }

}
```

归并排序算法的时间复杂度为$O(nlog_2n)$，由于需要用到和待排序记录等量的数组b存放结果，所以实现归并排序要增加一倍的存储空间。