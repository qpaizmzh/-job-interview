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



