# 使用Pipeline的好处和主从同步

## 使用Pipeline的好处

* Pipeline和Linux的管道类似
* Redis基于请求/响应模型，单个请求处理需要一一解答
* Pipe批量执行指令，节省多次IO往返的时间
* 有顺序依赖的指令建议分批发送

## Redis的同步机制

### 全同步过程

* Slave发送sync指令到Master
* master启动一个后台进程，将Redis中的数据快照保存到文件中
* Master将保存数据快照期间接收到的写命令缓存起来
* Master完成写文件操作后，将该文件发送给Slave
* 使用新的AOF文件替换掉就的AOF文件
* master将这期间收集的增量写命令发送给Slave端



