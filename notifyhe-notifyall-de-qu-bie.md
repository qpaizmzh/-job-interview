# notify和notifyAll\(\)的区别

* notify是随机选取一个处于等待池的线程进入锁池去竞争目标锁的使用
* notifyAll能唤醒全部处于等待池的线程进入锁池
* ![](/notify/1.png)
* ![](/notify/2.png)

* 使用Thread.yield\(\)方法会给线程调度一个当前线程愿意让出CPU使用的暗示，但是线程调度器肯呢个会忽略这个暗示



