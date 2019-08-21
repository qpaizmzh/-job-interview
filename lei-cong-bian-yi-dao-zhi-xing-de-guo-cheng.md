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



