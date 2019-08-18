# 如何使用Redis做异步队列

* ## 使用List作为队列，RPUSH作为生产消息，LPOP消费消息

  * 缺点：没有等待队列里有值就直接消费

  * 解决：可以通过在应用层引入sleep机制去调用LPOP重试，或者使用:BLPOP key \[key...\] timeout：阻塞直到队列有消息或者超时

## 



