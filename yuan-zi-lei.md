### \# 原子类（解决并发的使用的基本类型类）

### 原子类的包 AtomicXXX

* 底层使用的基本上是CAS算法， 底层会有一个循环，把当前要操作的对象中修改的值和重新从主内存中读取同样的值做比较，如果两个值一致，则进行操作，然后跳出循环

## CAS算法中的ABA问题

* ![](file://C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20191026105912283.png?lastModify=1572541285 "image-20191026105912283")



