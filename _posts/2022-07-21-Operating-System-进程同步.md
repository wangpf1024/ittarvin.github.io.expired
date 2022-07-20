---
layout: post
subtitle: 《操作系统》8小节： 系统调用，一般函数
tags: [操作系统]
comments: false
cover-img: /assets/img/IMG_1753.JPG
thumbnail-img: /assets/img/操作系统概论.jpeg
share-img: /assets/img/IMG_1482.JPG
---

在多道程序环境下，进程之间可能存在资源共享关系，相互合作关系。进程同步有两个任务，一是对具有资源共享关系的进程，保证诸进程以互斥的访问临界资源（必须以互斥方式访问的共享资源）。二是对具有相互合作关系的进程，保证相互合作的诸多进程协调执行。相互合作的进程可能同时存在资源共享的关系。

# 操作系统

##  进程同步

### 同步机制应遵循的准则

##### (1).空闲让进
当没有进程处于临界区时，表明临界区资源处于空闲状态，应允许一个请求进入临界区的进程理解进入自己的临界区，以有效的利用临界资源。

##### (2).忙则等待
当已有进程进入临界区时，表明临界资源正在被访问，因而其它试图进入临界区的进程必须等待，以保证临界资源的互斥访问。

##### (3).有限等待
对要求访问临界资源的进程，应保证在有限时间内能进入自己的临界区，以免进程陷入无限等待状态。

##### (4).让权等待
当进程申请不到共享资源的访问权限时候，应立即释放处理机，以免进程陷入“忙等”状态，浪费CPU资源。

### 信号量机制

#### 整型信号量机制
整型信号量时表示共享资源状态且只能有特殊原子操作改变的整型量。
其完成同步功能的原理：
定义一个整型变量，用整型变量值来标记资源的使用情况。如果整型量 $> 0$,说明有可用资源；如果整型量 $\leq 0$ ,说明资源忙，进程必须等待。
整型型号量的值只能通过两个特定的原子操作 wait 和 signal 来改变。

##### (1).整型信号量的 wait 和 signal 操作

```cpp
int s = 1;
//申请资源
wait(s){
    while s<= 0 do no - op;
          s = s -1;
}
//释放资源
signal(s){
    s = s + 1;
}
```

##### (2).整型信号量实现进程互斥访问

```cpp
int counter = 0;
//P1
{
    wait(mutex);
      counter = counter + 1;
    signal(mutex);
}
//P2
{
    wait(mutex);
      counter = counter + 1;
    signal(mutex);
}
```

##### (3).整型型号量实现进程的协调

```cpp
//p1 -> p2
parbegin
    begin p1;signal(s);end
    begin wait(s); p2;end
parend
```


#### 记录型信号量机制

##### (1).记录型信号量的数据类型

```cpp
Type semaphore = record
                 Value: integer;
                 L: list of process;
            end
```

##### (2).记录型号量的 wait 和 signal 操作

```cpp
procedure wait(s)
    var s : semaphore;
    begin
        s.value = s.value -1;
        if s.value < 0 then block(s.L);
    end.
        
prcedure signal(s) 
    var s : semaphore;
    begin
        s.value = s.value +1;
        if s.value <= 0 then wakeup(s.L);
    end.
```

##### (3).记录型号量实现进程互斥访问

```cpp
var s:semaphore;
s.value = 1;
Begin
    Repeat
        wait(s);
          Critical Section;
        signal(s);
          Remainder Section;
    Until false;
End
```

例：3个进程访问同一个临界资源时，利用记录型信号量实现互斥的情况。
 
3个进程 $p_1,p_2,p_3$ 访问同一个临界资源，互斥信号量为 s，s 的初始值为 1。
 
![操作系统-同步-记录型型号量.png](/assets/img/操作系统-同步-记录型型号量.png)
 
 
 

#### AND 型信号量机制


##### (1).AND 信号量机制的引入

一个进程在运行运行过程中往往需要申请多个共享资源，如果使用整型或记录型信号量机制，可能会出现因为申请资源的顺序不当导致进程死锁。

```cpp
processA:
wait(Dmutex);
wait(Emutex);

processB:
wait(Emutex);
wait(Dmutex);
```
进程A因为等待进程B释放临界资源E的访问权而被阻塞，进程B因为等待进程A释放临界资源D的访问权而被阻塞。

AND 信号量机制的基本思想是将进程在整个运行过程中所需要的所有资源一次性的全部分配给进程，等待改进使用完毕后再一起释放。只要还有一个资源不能分配给该进程，其它所有可能为之分配的资源也不能分配给它。

##### (2).AND 信号量机制的实现

为不同的共享资源设定相应信号量 $s_1,s_2,\dots,s_n$ , 调用 Swait 操作申请资源，调用 S signal 操作释放资源。

```cpp
Swait(s1,s2,....,sn)
    if s1 >= 1 and .... sn >=1
            for i=1 to n do si = si -1
          end for
    else
        //把进程插入 Si 阻塞队列，并把计数器的值赋为 Swait 的起始地址。
    end if
        
Ssignal(s1,s2,....,sn)
         for i =1 to n do
            Si = Si + 1;
         //将阻塞在 Si 队列中的进程唤醒 ，插入就绪队列  
         end for   
```


####  总结

| 整型信号量机制                              | 记录型信号量机制        | AND 型信号量机制 |
|--------------------------------------|-----------------|------------|
| 整型型号量的值只能由 wait 和 signal 操作完成。       |                 |            |
| wait 和 signal 操作都是原子操作。              |                 |            |
| 原子操作可以通过关中断来实现。                      |                 |            |
| 不同的资源对应不同的信号量，并不是系统中所有的资源都用同一个信号量表示。 |                 |            |
| 实例：Linux 2.4 中的自旋锁 SpinLock          | 实例： semaphore.h |            |
|                                      |                 |            |
 