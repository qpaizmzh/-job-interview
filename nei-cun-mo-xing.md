## 内存模型

## 地址空间的划分

* 内核空间
* 用户空间

## JVM内存模型-JDK8

* 线程私有：程序计数器、虚拟机栈、本地方法栈

  * ### 程序计数器（Program Counter Register）

    * 当前线程所执行的字节码指示器（逻辑）
    * 改变计数器的值来选取下一条需要执行的字节码指令
    * 和线程是一对一的关系即“线程私有”
    * 对Java方法技术，对native方法记为“undefined”
    * 不会发生内存泄漏
  * Java虚拟机栈（Stack）
    * Java方法执行的内存模型
    * 包含多个栈帧
    * 局部变量表和操作树栈的区别
      * 局部变量表：包含方法执行过程中的所有变量
      * 操作树栈：入栈、出栈、复制、交换、产生消费变量
      * ![](/模型/1.png)
      * ![](/assets/3.png)
      * 题外话：递归为什么会引发java.lang.StackoverflowError异常？
        * 递归过深，栈帧数超出虚拟栈深度，解决方法就是使用循环代替
      * 同样的一个虚拟机栈对应一个线程，如果虚拟机栈过多，线程过多并且没有足够的内存使用就会抛出内存溢出的异常

* 所有线程共享：MetaSpace,Java堆

  * 元空间（MetaSpace）和永久代（PermGen）的区别
    * 元空间使用本地内存，而永久代使用的是JVM的内存
  * 元空间相比永久代的优势
    * 字符串常量池存在永久代中，容易出现性能问题和内存溢出
    * 类和方法的信息大小难以确定，给永久代大小指定带来困难
    * 永久代会为GC带来不必要的复杂性
    * 方便HotSpot与其他JVM如Jrockit的集成
  * Java堆（Heap）
    * 对象实例的分配区域
    * GC管理的主要区域

* 常考题目

  * JVM三大性能调优参数-XMS -XMX -XSS的含义
    * -XSS 规定了每个线程虚拟栈的大小，一般是256k足够
    * -xms 堆的初始值
    * -xmx 堆的最大值 一般建议设置和堆的初始值一致，不一致导致内存自动扩展时可能影响内存稳定
  * Java内存模型中堆和栈的区别
    * 内存分配策略
      * 静态储存：编译时确定每个数据在运行时的存储空间需求，这个栈数固定的分配一般不能用于像是递归这种调用
      * 栈式储存：数据区需求在编译时位未知，运行时模块入口前确定
      * 堆式储存：编译时或者运行时模块入口都无法确定，动态分配
    * 联系
      * 引用对象、变量时，栈定义变量保存堆中目标的首地址
    * 管理方式
      * 栈自动释放，堆需要GC回收
      * 空间大小：栈比堆小
      * 碎片相关：栈产生的碎片远小于堆
      * 分配方式：栈支持静态和动态分配，而堆只支持动态分配
      * 效率：栈的效率比堆高
    * 不同JDK版本之间的intern\(\)方法的区别（这个方法是用于字符串加入到字符串常量池用的）
      * ![](/模型/4.png)
      * ## ![](/模型/5.png)JMM模型用图

  
      ![](file://C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20191025155311481.png?lastModify=1572540845 "image-20191025155311481")

      ### 同步八种操作

      * ![](file://C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20191025160924216.png?lastModify=1572540845 "image-20191025160924216")

      ![](file://C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20191025160934289.png?lastModify=1572540845 "image-20191025160934289")

      ![](file://C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20191025161021140.png?lastModify=1572540845 "image-20191025161021140")

      ![](file://C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20191025161212455.png?lastModify=1572540845 "image-20191025161212455")

      ![](file://C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20191025161222848.png?lastModify=1572540845 "image-20191025161222848")

      ![](file://C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20191025161253110.png?lastModify=1572540845 "image-20191025161253110")

  
      ![](file://C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20191025161320327.png?lastModify=1572540924 "image-20191025161320327")



