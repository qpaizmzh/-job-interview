# 插入排序

* 插入排序的本质是将a\[i\]和前面的a\[i-1\]两两比较交换，把最小的值放到最左边

```java
package com.agth;

public class InsertionTest {
    public static void sort(int[] a) {
        //核心的思想是将前面的N个数字进行排序交换，像是倒序的冒泡
        int j = a.length;
        for (int i = 1; i < j; i++) {
            for (int k = j; k > 0; k--) {
                if (a[k - 1] > a[k]) {
                    int temp = a[k - 1];
                    a[k - 1] = a[k];
                    a[k] = temp;
                }
            }
        }
    }
}

```



