# 谈谈ClassLoader

## 类从编译到执行的过程

* 编译器将类.java源文件编译为Robot.class字节码文件
* ClassLoader将字节码转换为JVM中的Class&lt;Robot&gt;对象
* JVM利用Class&lt;Robot&gt;对象实例化为Robot对象

## ClassLoader的种类

* BootStrapClassLoader：C++编写，加载核心库java.\*
* ExtClassLoader:Java编写，加载扩展库javax.\*
* AppClassLoader:Java编写，加载程序所在目录
* 自定义ClassLoader：Java编写，定制化加载

## 谈谈ClassLoader

ClassLoader在Java中有着非常重要的作用，主要工作在Class装载的加载阶段，其主要作用是从系统外部获得Class二进制数据流。它是Java的核心组件，所有的Class都是由ClassLoader进行加载的，ClassLoader负责通过将Class文件里的二进制数据流装载进系统，然后交给Java虚拟机进行连接，初始化等操作

