# 并发实践

* 使用本地变量（即方法变量）

* * ![](file://C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20191028234610860.png?lastModify=1572543734 "image-20191028234610860")
* 使用不可变类，可以降低同步锁的数量

* 最小锁的作用域范围：了解一下：![](file://C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20191028235139972.png?lastModify=1572543734 "image-20191028235139972")

* 使用线程池的Executor,而不是直接new Thread执行

* 宁可使用同步也不要使用线程的wait和notify，有synchronized和reentrianLock代替

* 使用BlockingQueue实现生产-消费模式

* 使用并发集合而不是加了锁的同步集合

* 使用semephore创建有界的访问，对于资源有限的访问，必须要控制并发的线程数

* 宁可使用同步代码块，也不使用同步的方法，比锁定方法灵活

* 避免使用静态变量，如果必须使用尽量靠上final关键字，因为这个共享变量容易引起并发的问题

## HashMap和CurrentHashMap

* hashmap中的链表长度超过8以后，就会由链表变成红黑树

* 宁愿hashmap尽量不要用于并发，容易出现因在扩容的时候出现死循环

* ConcurrentHashMap在java7版本以前使用个的是分段锁，根据hash值来决定某个HashEntry数组的索引值

* 多线程注意知识点：

![](file://C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20191029111820220.png?lastModify=1572543734 "image-20191029111820220")

## 高并发处理思路和手段

* 扩容有两种，要么服务器架构群不变，提升程序处理的效率（代码优化），也叫垂直扩展，而另一种是增加硬件，使用更多的硬件处理程序，也叫水平扩展

* 数据库是共享资源，被其他线程利用的情况多，扩展也就是两种思路：读操作的扩展:memcache，redis,CDN等缓存，写操作扩展的话就使用：Cassandra,Hbase等的工具

## 高并发的处理场景演示

* 工具有guava cache\(本地缓存\)，memcache\redis\(分布式缓存\)

* redis.cn教程网站

## 高并发场景下缓存常见问题

* 缓存一致性

  * 就是缓存和数据库数据不一致的问题

* 缓存并发问题（过多的直接冲击数据库获取数据可能会造成崩溃，一般情况下是获取锁，然后更新缓存，然后由缓存被线程获取数据，不需要直接多次访问数据库）

  * ![](file://C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20191029150704340.png?lastModify=1572543734 "image-20191029150704340")

* 缓存穿透问题

  * 高并发下访问一个key，没有被命中，处于容错性的问题，会尝试访问后端的数据库，结果返回的同样是null,这样大量冲击数据库请求，要么缓存空对象或者空集合，减少冲击

* 缓存的雪崩现象

  * 就是大量的请求穿过缓存到数据库，导致数据库崩溃

* 缓存实战的一个例子：

```
在内存中通过guava cache缓存了最近几分钟的所有symbol的最新数据，key: 股票+HHmm(美东时间)， value：一条分时数据，每次推送分时数据过来时，根据股票及分时数据时间进行cache，过期数据（很久以前或非最新数据）或脏数据（相关数据为NaN）直接抛弃，然后通过ScheduledExecutorService维护一个1分钟1执行的job去将最近几分钟的分时数据更新到redis中。这样既解决了分时数据有延迟的问题，又保证了每分钟的redis更新数据量。
```

## 高并发下的消息队列

* 特点

  * 业务无关，制作消息分发

  * FIFO： 先排队先到达

  * 容灾：节点的动态增删和消息的持久化

  * 性能：吞吐量的提升，系统内部通信效率高

## 高并发下的应用拆分

* 应用拆分一般在业务场景下的需要或者是性能的需要进行拆分的，因为有可能遇到不同模块的场景下处理数据的方式、压力不进相同，需要单独分开一个模块才能更好的优化相应的性能

* 应用之间的通信：RPC还是消息队列

* 应用间的数据库设计：每个应用都有独立的数据库

* 避免失误操作跨应用

## 应用限流（了解）

* 计数器法

* 滑动窗口

* 漏桶算法

* 令牌桶算法

## 服务降级和服务熔断

* 降级分类：人工降级和自动降级

* 好用的工具：Hytrisx

## 数据库切库、分库、分表

* 单个库的数据量太大-- 多个库

  * 搜索自定义数据库切库（慕课）

* 单个服务器的压力过大，读写瓶颈--多个库

* 单个表的数据量过大，分表

  * 横向分表和纵向分表

  * mybatis分表的插件：shardbatis

## 高可用的手段

* 任务调度系统分布式：elastic-job+zookeeper

* 主备切换：apache curator +zookeeper分布式锁实现

* 监控报警机制


