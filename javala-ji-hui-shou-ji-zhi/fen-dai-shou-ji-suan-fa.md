# 分代收集算法

* 垃圾回收算法的综合，会按照对象生命周期的不同划分区域以采用不同的垃圾回收算法，以提高JVM的回收效率
* GC分类：

  * Minor GC

  * Full GC
* 年轻代：尽可能快速地收集掉生命周期短的对象（适合复制算法）

  * Eden区

  * 两个Survivor区

  * ![](/分代/1.png)
* 对象如何晋升到老年代

  * 经历一定Minor次数依然存活的对象
  * Survivor区中存放不下的对象
  * 新生成的大对象（-XX：PretenuerSizeThreshold）
* 常用的调优参数

  * -XX：SurvivorRatio：Eden和Survivor的比值，默认8：1
  * -XX:NewRatio:老年代和年轻代内存大小的比例
  * -XX：MaxTenuringThreshold:对象从年轻代晋升到老生代经过GC次数的最大阀值
* 老年代：存放生命周期较长的对象

  * 使用标记-清除和标记-整理算法回收内存
  * Full GC比Minor GC慢，执行频率低
  * 出发Full GC的条件
    * 老年代空间不足
    * 永久代空间不足（1.8及以后没有永久代）
    * CMS GC时出现promotion failed\(这个出现在Minor GC回收，survivor区空间不足，对象放入老年代，老年代也是空间不足)，concurrent mode failed（这个出现在CMS GC执行的时候同时有对象放入老年代，老年代空间不足导致的异常）
    * Minor GC晋升老年代的平均大小大于老年代的剩余空间
    * 调用System.gc\(\)
    * 使用RMI进行RPC或者管理的JDK应用，每小时执行一次Full GC
* stop the world
  * JVM由于要执行GC而停止了应用程序的执行
  * 任何一种GC算法都会发生
  * 多数GC优化通过减少Stop-the-world发生的时间来提高程序性能



