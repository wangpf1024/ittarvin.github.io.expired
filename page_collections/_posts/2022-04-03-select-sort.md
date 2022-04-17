---
layout: post
title:  "直接选择排序"
date:   2022-04-03 17:37:49 +0800
categories: data/structure
tags: "数据结构与算法"
---

{% include MathJax.html %}

直接选择排序算法的思想：在第 **i** 次选择操作中，通过 **n - i** 次键值比较，从 **n - i + 1** 个记录中选出键值最小的记录，并第  **i （1 <= i <= n -1 ）**个记录交换。

~~~java
/**
 * 待排序数组 R[0] 作为岗哨
 */
static int[] unSortR = {72,26,57,88,42,80,73,48,60};


/**
 * 输出排序数组元素
 */
public static void sysOutSortR(){
    for (int i = 0; i < unSortR.length; i++) {
        System.out.print(unSortR[i]);
        System.out.print(",");
    }
}


public static void selectSort(int[] R,int n){

    int min,i,j;

    for ( i = 0; i <=n-1; i++) {

        min = i;

        for (j = i + 1;  j <= n ;j++) {
            if(R[j] < R[min]) min = j;
        }

        if(min != i){
            int tmp = R[i];
            R[i] = R[min];
            R[min] = tmp;
        }
    }

}
~~~

{% include list_posts.html %}
{% include list_tags.html %}