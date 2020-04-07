# 冒泡算法

* 冒泡算法的本质是相邻两个元素两两比较交换，然后把最大的元素放到最右边之后，重新进行遍历，找到第二大元素，一直循环

```java
package com.agth;

public class BubbingTest {
    //冒泡排序的本质是相互两两比较，如果有出入，就进行交换，然后继续进行比较，把最大的放到最右边
    public static void sort(int[] a) {
        int j = a.length - 1;
        for (int i = 0; i < a.length - 1; i++) {
            for (int k = 0; k < j; k++) {
                if (a[k] > a[k + 1]) {
                    int temp = a[k];
                    a[k] = a[k + 1];
                    a[k + 1] = temp;
                }
            }
            j--;
        }
    }

    public static void main(String[] args) {
        int[] a = {3,2,1,6,5,4,9,7,8};
        sort(a);
        for (int i = 0; i < a.length; i++) {
            System.out.println(a[i]);
        }
    }
}

```



