# 平台无关性怎么实现？

原生反编译的命令：javap -c com.abc.cdf.ehg.class 其中com后面的是class文件的全路径

原生反编译例子（字节码）：在非静态方法中，

aload\_0 表示对this的操作，在static 方法中，

aload\_0表示对方法的第一参数的操作。

```java
Compiled from "ChatResp.java"
public class com.nobody.hr.bean.ChatResp {
  public java.lang.String getMsg();
    Code:
       0: aload_0
       1: getfield      #1                  // Field msg:Ljava/lang/String;
       4: areturn

  public void setMsg(java.lang.String);
    Code:
       0: aload_0
       1: aload_1
       2: putfield      #1                  // Field msg:Ljava/lang/String;
       5: return

  public java.lang.String getFrom();
    Code:
       0: aload_0
       1: getfield      #2                  // Field from:Ljava/lang/String;
       4: areturn

  public void setFrom(java.lang.String);
    Code:
       0: aload_0
       1: aload_1
       2: putfield      #2                  // Field from:Ljava/lang/String;
       5: return

  public com.nobody.hr.bean.ChatResp();   //这里是编译时默认给一个无参的构造器
    Code:
       0: aload_0    //this对象
       1: invokespecial #3                  // Method java/lang/Object."<init>":()V
       4: return

  public com.nobody.hr.bean.ChatResp(java.lang.String, java.lang.String);
    Code:
       0: aload_0
       1: invokespecial #3                  // Method java/lang/Object."<init>":()V
       4: aload_0
       5: aload_1
       6: putfield      #1                  // Field msg:Ljava/lang/String;
       9: aload_0
      10: aload_2
      11: putfield      #2                  // Field from:Ljava/lang/String;
      14: return
}
```

![](/平台/1.png)Java源码首先被编译成字节码，再由不同平台的JVM进行解析，Java语言在不同的平台上运行时不需要进行重新的编译，Java虚拟机在执行字节码的时候，把字节码传换成具体平台上的机器指令

