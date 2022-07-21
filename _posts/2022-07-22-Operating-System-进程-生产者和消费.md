---
layout: post
subtitle: 《操作系统》 10小节： 系统调用，一般函数
tags: [操作系统]
comments: false
cover-img: /assets/img/IMG_1753.JPG
thumbnail-img: /assets/img/操作系统概论.jpeg
share-img: /assets/img/IMG_1482.JPG
---

生产者-消费者问题是相互合作关系的一种抽象。

# 操作系统

##  进程同步经典的问题

### 生产者-消费者问题的描述

#### 问题描述

生产者进程生成消息，并将消息提供给消费者进程消费。在生产者和消费者进程之间设置一个具有n个缓冲区的缓冲池，生产者进程可以将它生成的消息放入缓冲池的一个缓冲区中，消费者进程可以从一个缓冲区中取的一个消息消费。任意两个进程互斥访问公共缓冲池。当缓冲池为空，没有可以消费的消息时，消费者进程必须阻塞等待。当缓冲池装满消息，没有空闲缓冲区时，生产者进程必须阻塞等待。

#### 需要解决的问题

1. 实现任意两个进程对缓冲池的互斥访问。
2. 实现对生产进程，消费进程的“协调”，即缓冲区由消息时消费者才能执行去消息操作。 无消息时，阻塞消费者进程。缓冲池有空闲缓冲区时，生产者进程才能执行放消息的操作。无空间缓冲区时，阻塞生产者进程。

####  信号量量的设置

1. 设置一个互斥信号量 mutex ，用于实现对公共缓冲池的互斥访问，初始值为 1。
2. 设置两个资源信号量 ，分别表示可用资源数。
        empty: 表示缓冲区的空缓冲区数，初始值为 n。
        full: 表示装有消息的缓冲区数量，初始值为 0（一个缓冲区中放一个消息）。
        
####  伪代码实现

```cpp
//生产者进程同步代码描述 
Producer:
   begin
 repeat
   ...
   produce an item in nextp;
   wait(empty);
   wait(mutex);
   buffer(in) = nextp;
   in = (in + 1) mod n;
   signal(mutex);
   signal(full);
until false;
end
```

```cpp
//消费者进程同步代码描述 
Consumer:
   begin
 repeat
   ...
   wait(full);
   wait(mutex);
   nextc = buffer(out);
   out = (out + 1) mod n;
   signal(mutex);
   signal(empty);
   consume item in nextc;
until false;
end
```

####  说明

1. wait 和 signal 操作必须成对出现。

2. wait 操作的顺序不能颠倒。必须先对资源信号量（empty 和 full ）进行wait操作，然后对互斥信号量进行wait 操作。

3. 用记录型信号量机制解决生产者-消费值问题，对具有合作关系的进程，提供了解决问题的模型。