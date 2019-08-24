# notify和notifyAll\(\)的区别

* notify是随机选取一个处于等待池的线程进入锁池去竞争目标锁的使用
* notifyAll能唤醒全部处于等待池的线程进入锁池
* ![](/notify/1.png)
* ![](/notify/2.png)



