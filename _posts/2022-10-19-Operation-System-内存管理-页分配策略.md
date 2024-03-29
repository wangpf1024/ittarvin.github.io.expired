---
layout: post
subtitle: 《操作系统》 22小节： 页分配策略
tags: [操作系统]
comments: false
cover-img: /assets/img/IMG_1753.JPG
thumbnail-img: /assets/img/操作系统概论.jpeg
share-img: /assets/img/IMG_1482.JPG
---


**1.只要装进进程的一部分分页就可以运行进程，那么究竟为进程分配多少个页框，在内存中装入进程的多少个页呢？**


**2.操作系统在需要请求调用并进行页置换时，是从缺页进程本身页框中选择淘汰页，还是从系统中所有进程中选择淘汰的页？**


**3.采用什么样的算法为不同进程分配页框？系统如何在多个进程之间合理分配页框数？**

# 操作系统

##  内存管理

### 最少页框数

最少页框数，是指能保证进程正常运行所需要的最少的页框数。如果系统为进程分配页框数小于这个值，进程将无法正常运行。

保证进程正常运行所需要的最小页框数与进程的大小没有关系，它与计算机的硬件结构有关系，取决于指令格式，功能和寻址方式。


|  操作码 |  内存地址 |



### 页分配和置换策略

**固定分配局部置换**

当进程创建时为进程分配一定数量的页框，在进程运行期间，进程拥有的页框数不在改变。当进程发生缺页时，系统从该进程的内存中的页中选择一页换出，然后再调入请求的页，以保证分配给该进程的内存空间保持不变。

**可变分配全局置换**

这是在操作系统中被广泛使用的策略。采用这种策略时，先为系统中的每个进程分配一定数量的页框，同时，操作系统保持一个空闲队列。

当某个进程发生缺页时，由系统从空闲页框队列中获取一个页框分配给该进程，并将欲调入的缺页装入其中。任何产生缺页的进程都可以由系统获取新的页框，以增加本进程在物理内存中的页数。

当系统总空闲页框数小于一个规定的阀值时，操作系统选择从内存中选择一些页调出，以增加系统空闲框数量，调出的页可能是系统的任何一个进程的页。


**可变分配局部置换**

进程创建时，为进程分配一定数量的页框。当进程发生缺页时，只允许从该进程在内存的页中选出一个页换出。只有当进程频繁发生缺页时，操作系统才会为进程追加页框，以装入更多的进程页，直到该进程的缺页率降低到适度程度。反之，若一个进程在运行过程中缺页率特别低，则可在不引起进程缺页率明显增加的前提下，适当减少该进程的页框数。

### 分配算法

操作系统为进程分配的页框数通常都是大于最小页框数的，究竟为每个进程分配多少页框，可通过分配算法来确定。

**平均分配算法**

采用平均分配算法，如果一个系统中有 n 个进程，m 个可供分配的内存页框，则为每个进程分配 $INT[m/n]$ 个页框，其余的 $MOD[m/n]$ 个页框可以放入空闲页框缓冲池中。

**按比例分配算法**

平均分配算法为进程分配页框的缺点是算法不考虑进程规模，可能使大进程分配到的进程和小进程一样多。大进程能进入内存的页数占本进程页总是的比例远远小于小进程，大进程可能更频繁的缺页。可以采用按比例分配：
$$
为进程分配的页框数 = 进程页数/所有进程页数的总和 \times 页框数
$$

**考虑优先权的分配算法**

这种算法的思想时有限为优先权较高的进程分配较多的页框数，为低优先级的进程分配较少页框数。



