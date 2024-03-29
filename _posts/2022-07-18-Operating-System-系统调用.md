---
layout: post
subtitle: 《操作系统》8小节： 系统调用，一般函数
tags: [操作系统]
comments: false
cover-img: /assets/img/IMG_1753.JPG
thumbnail-img: /assets/img/操作系统概论.jpeg
share-img: /assets/img/IMG_1482.JPG
---


系统调用把用户从学习硬件设备的低级编程特性中解放出来，并极大的提高了系统的安全性。

# 操作系统

##  操作系统内核

### 系统调用
系统调用是一群预先定义好的模块，它们提供一条管道让应用程序或一般用户能由此得到核心程序服务。

### 系统调用与一般函数
1. 什么是用户态执行？
> 用户空间是指用户进程所处的地址空间，一个用户的进程不能访问其它进程用户空间，只有系统程序才能访问其它用户空间。当CPU执行用户空间代码是，称该进程在用户态执行。
2. 什么是系统态执行？
> 系统空间是指含有一切核心代码的地址空间，当CPU执行核心代码时，称进程处于系统态执行。
3. 系统调用与一般函数的区别
> 系统调用运行在系统态，一般函数调用运行在用户态。
> 系统调用与一般函数调的执行过程不同，系统调用时，当前进程被中断，由系统找到相应的系统调用子程序，并在系统态下执行，执行结果返回进程。
> 系统调用要进行“[中断处理](/2022-07-15-Operating-System-系统内核)”，比一般函数调用多了一些系统开销。

### 普通函数的执行过程实例

例如：调用 C 的库函数，求一个整数的绝对值。

```cpp
Main(){
    int a;
    ...;
    a = abs(a);
    ...;
}
```
当执行求绝对值的函数调用时，程序执行的控制流只需要转移到求决定值的入口处，如果下图所示就是跳转到地址为 100 的内存单元开始执行一段求绝对值的子程序。在函数的调用前后，进程都处于用户态，请求绝对值的函数和用户程序一样处于用户空间。

![Image not found: /assets/img/操作系统-系统调用-一般函数.png](../assets/img/操作系统-系统调用-一般函数.png){: .mx-auto.d-block :}

### 系统调用的执行过程实例

例如：调用 getpid() 求当前进程内部标识符的用户程序如下。

```cpp
Main(){
    int i =getpid();
    printf("%d",i)
}
```
![操作系统-系统调用.png](../assets/img/操作系统-系统调用.png){: .mx-auto.d-block :}
为了把系统调用号与相应的系统调用实现程序关联起来，Linux 内核利用一个系统调用分配表（Dispatch Tabl;e）。这个表存放在 system_call 数组中，有NR_syscalls 个表项（linux 2.6.11 内核的这个值是 289）。系统调用分派表的第n个表项包括系统调用号为 n 的系统调用实现程序的地址。
getpid() 对应的系统调用号是 _NR_getpid ，所以在系统调用分派表的第  _NR_getpid 个表项中存在 sys_getpid 程序的起始地址。

### 系统调用的类型
1. 进程控制类系统调用。（创建，撤销进程；获取，改变进程属性） 
2. 文件操作类系统调用。（创建文件，删除文件，打开文件，关闭文件，读写文件）
3. 设备管理类系统调用。（请求，释放设备）
4. 通信类系统调用。（打开，关闭了连接，交换信息）
5. 信息维护类调用。（返回系统当前日期，时间，用户数，版本号，磁盘空间，空闲内存等信息）