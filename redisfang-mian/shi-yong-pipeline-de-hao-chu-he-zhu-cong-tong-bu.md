# 使用Pipeline的好处和主从同步

## 使用Pipeline的好处

* Pipeline和Linux的管道类似
* Redis基于请求/响应模型，单个请求处理需要一一解答
* Pipe批量执行指令，节省多次IO往返的时间
* 有顺序依赖的指令建议分批发送



