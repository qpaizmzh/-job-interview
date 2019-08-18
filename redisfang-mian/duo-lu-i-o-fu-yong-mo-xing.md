# 多路I/O复用模型

## FD: File Descriptor，文件描述符

一个打开的文件通过唯一的描述符进行引用，该描述符是打开文件的元数据到文件本身的映射

## 传统的阻塞I/O模型

当使用read或者write对某个文件的描述符进行操作时，如果当前FD不可read或者不可write，整个redis服务就无法对其他的请求作出响应

## 多路I/O服用模型常用函数select

* select可以监控多个文件描述符的可读可写情况，有可用的可读可写的描述符就会返回，而程序就不会被I/O堵塞，从而可以继续做其他的事情
* redis优先选择时间复杂度是O\(1\)的I/O多路复用函数作为底层的实现
* 以时间复杂度为O\(N\)的select作为保底
* 基于react设计模式监听I/O事件


