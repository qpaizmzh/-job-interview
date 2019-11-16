# 流的使用

## 流操作和集合操作对比

* 直观对比一下集合操作和流操作的编码的数据量

```java
package com.agth;

import java.util.ArrayList;
import java.util.Comparator;
import java.util.List;
import java.util.concurrent.atomic.AtomicReference;
import java.util.stream.Collectors;

public class SteamVsList {
    //实例
    /**
     * 看看购物车有什么商品
     * 图书类商品都买
     * 其余的商品买两件最贵的
     * 只需要两件商品的名称和总价
     */

    /**
     * 首先以原始集合操作实现的需求
     */
    public void oldCartHandle() {
        List<Skt> cartSkuList = new ArrayList<>();//模拟已经获取了相当的数据

        /**
         * 打印所有商品
         */
        for (Skt s :
                cartSkuList) {
            System.out.println(s);
        }
        /**
         *电子类的过滤掉
         */
        List<Skt> bookSkuList = new ArrayList<>();//这个是用来存储被筛选过的列表
        for (Skt s :
                cartSkuList) {
            if (!StringCatogoryEnum.electronic.equals(s.getCatogory())) {
                bookSkuList.add(s);
            }
        }

        /**
         *排序
         */
        bookSkuList.sort(new Comparator<Skt>() {
            @Override
            public int compare(Skt o1, Skt o2) {
                return 0;
            }
        });

        /***
         * 选择前两个
         */
        List<Skt> top2 = new ArrayList<>();
        for (int i = 0; i < 2; i++) {
            top2.add(bookSkuList.get(i));
        }

        /***
         * 求两件商品的总价
         */
        Double money = 0.0;
        for (Skt skt :
                top2) {
            money += skt.getMoney();
        }


        /***
         * 获取两件商品的名称
         */
        List<String> top2Name = new ArrayList<>();
        for (Skt skt :
                top2) {
            top2Name.add(skt.getName());
        }

        /***
         * 打印输入的结果
         */
        System.out.println(result);
        System.out.println(money);
    }


    /***
     * 以Steam流的形式实现
     */
    public void newCartHandle() {
        //用于线程安全
        AtomicReference<Double> doubleAtomicReference = new AtomicReference<>(Double.valueOf(0.0));

        List<Skt> cartSkuList = new ArrayList<>();//模拟已经获取了相当的数据


        cartSkuList.stream()
                //遍历所有的商品
                .peek(skt -> System.out.println(skt))
                /**
                 *电子类的过滤掉
                 */
                .filter(skt -> StringCatogoryEnum.electronic.equals(skt.getCatogory()))
                /***
                 * 排序
                 */
                .sorted(Comparator.comparing(Skt::getCatogory).reversed())
                /***
                 * 选择前两个
                 */
                .limit(2)
                /***
                 * 累加商品的金额
                 */
                .peek((skt -> doubleAtomicReference.set(doubleAtomicReference.get()+skt.getMoney())))
                /***
                 * 获取商品的名称
                 */
                .map(sku->sku.getName)
                /***
                 * 收集上面的名字的结果的
                 */
                .collect(Collectors.toList());

        /***
         * 打印输入的结果
         */
        System.out.println(result);
        System.out.println(money);
    }

}
```

* ## ![](/高效编程/流的组成.png)流的使用
* ## ![](/高效编程/流的使用.png)

## 流的创建

* 由值创建流
* 由数组创建流
* 由文件生成流
* 由函数生成流（无限流）



