# 使用Pipeline的好处和主从同步

## 使用Pipeline的好处

* Pipeline和Linux的管道类似
* Redis基于请求/响应模型，单个请求处理需要一一解答
* Pipe批量执行指令，节省多次IO往返的时间
* 有顺序依赖的指令建议分批发送

## Redis的同步机制

### 全同步过程

* Slave发送sync指令到Master
* master启动一个后台进程，将Redis中的数据快照保存到文件中\(也就是BGSAVE\)
* Master将保存数据快照期间接收到的写命令缓存起来
* Master完成写文件操作后，将该文件发送给Slave
* 使用新的AOF文件替换掉就的AOF文件
* master将这期间收集的增量写命令发送给Slave端

### 增量同步过程

* Master接收到用户的操作指令，判断是否需要传播到Slave
* 将操作记录追加到AOF中
* 将操作传播到其他的Slave：1、对其主从库 2、往响应缓存写入指令
* 将缓存中的数据发送给Slave

### Redis Sentinel

解决主从同步Master宕机后的主从切换问题：

* 监控：检查主从服务器是否运行正常
* 提醒：通过API向管理员或者其他应用程序发送故障通知
* 自动故障迁移：主从切换



