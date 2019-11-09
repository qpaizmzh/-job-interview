# 冒泡算法

* 冒泡算法的本质是相邻两个元素两两比较交换，然后把最大的元素放到最右边之后，重新进行遍历，找到第二大元素，一直循环

```java
package com.agth;

import java.util.Collections;
import java.util.Random;

public class Quick {

    //用来随机打乱数组
    public static void shuffle(Comparable[] arr) {
        Random rand = new Random();
        int length = arr.length;
        for (int i = length; i > 0; i--) {
            int randInd = rand.nextInt(i);
            exch(arr, randInd, i - 1);
        }
    }


    //比较大小的方法
    private static boolean less(Comparable v, Comparable w) {
        return v.compareTo(w) < 0;
    }

    //交换值
    private static void exch(Comparable[] a, int i, int j) {
        Comparable tr = a[i];
        a[i] = a[j];
        a[j] = tr;
    }

    //打印值
    private static void show(Comparable[] a) {
        for (Comparable instance :
                a) {
            System.out.println(instance);
        }
    }

    //测试是否有序
    private static boolean issorted(Comparable[] a) {
        for (int i = 0; i < a.length - 1; i++) {
            if (less(a[i], a[i + 1])) {
                return false;
            }
        }
        return true;
    }


    private static int partition(Comparable[] a, int lo, int hi) {
        //将数组切分为a[lo...-1],a[i],a[i+1...hi]

        int i = lo, j = hi + 1;//左右扫描的指针
        Comparable v = a[lo];//切分的元素
        while (true) {
            //扫描左右，检查元素是否结束并交换元素
            while (less(a[++i], v)) if (i == hi) break;
            while (less(v, a[--j])) if (j == lo) break;
            if (i >= j) break;
            exch(a, i, j);
        }
        exch(a, lo, j);//将V=a[j]放入正确的位置，这是最后的一步
        return j;   //将a[lo..j-1]<=a[j]<=a[j+1....hi]成立

    }

    public static void sort(Comparable[] a) {
        shuffle(a); //消除对输入的依赖
        sort(a, 0, a.length - 1);
    }

    private static void sort(Comparable[] a, int lo, int hi) {
        if (hi < lo) return;

        int j = partition(a, lo, hi);//切分,使用该方法将一个合适的对比的数放到一个指定的位置
        //调用回去反复切割
        sort(a, lo, j - 1);
        sort(a, j + 1, hi);
    }
}

```



