# 事务隔离级别以及各级别下的并发访问问题

## 事务并发访问引起的问题以及如何避免

* 更新丢失--mysql所有事务隔离级别在数据库上均可避免
  * ![](/事务隔离/1.png)
* 脏读——READ-COMMITED事务隔离级别以上可以避免
* 不可重复读——REPEATABLE-READ事务隔离级别以上可以避免

* 幻读——SERIALIZABLE事务隔离级别可以避免

  * ![](/事务隔离/2.png)

## InnoDB下可重复读隔离级别下如何避免幻读

* 表面原因：快照读（非阻塞读）--伪MVCC
* 内在：next-key锁（行锁+gap锁）

### 当前读和快照读

* 当前读就是加了锁的增删改查语句，叫当前读的原因是它是读取的数据最新的版本
* 快照读就是不加锁的非阻塞读（只在非串行化隔离级别才可以快照读）

### 对主键索引或者唯一索引会引用Gap锁吗？

* 如果where条件全部命中，则不会用Gap锁，只会加记录锁
* 如果where条件部分命中或者都不命中，则会加Gap锁
* gap锁会用在非唯一索引或者不走索引的当前读中



