# 选择排序

* 选择排序的本质是在每个循环的内循环中找到最小的数字（升序），然后和外循环的指定索引进行值的交换

```java
package com.agth;

public class OptionTest {

    public static void sort(int[] a, int low, int high) {
        for (int i = 0; i < high; i++) {
            int index = i;
            for (int k = i + 1; k < high; k++) {
                if (a[i] > a[k]) {
                    index = k;
                }
            }
            int temp = a[i];
            a[i] = a[index];
            a[index] = temp;
        }
    }


    public static void main(String[] args) {
        int[] a = new int[]{3, 2, 1, 4, 5};
        sort(a, 0, a.length);
        for (int i = 0; i < a.length; i++) {
            System.out.println(a[i]);
        }
    }
}

```



