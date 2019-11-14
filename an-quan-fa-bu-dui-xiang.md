# 安全发布对象

* 有时候像是这种的本来想封装的对象因为某些方法导致其变量被公布出去能够直接使用：

* ```
  public class test {
      private String[] abc = {"1", "2", "23", "4"};

      public String[] getAbc() {
          return abc;
      }

      public static void main(String[] args) {
          String[] abc = new test().getAbc();
          System.out.println(abc[3]);
          abc[3] = "d";
          System.out.println(abc[3]);

      }
  }
  ```
* 类似的情况还有map类型的，如果只是添加了给map变量添加了final关键字，但是还是不会对里面的内容产生不可变特性，依旧可以修改map里面对应的值，要想改变这种情况，要么创建该map的时候给这个value类型添加final关键字，要么使用Collections.unmodifiedMap\(Map map\)方法

* ## 解决方案：
* 在静态初始化函数中初始化一个对象引用

* 将对象的引用保存到volatile类型域或者是AtomicReference对象中

* 将对象的引用保存到某个正确的对象的final类型中

* 将对象的引用保存到一个由锁保护的域中

* **详情可以查看线程安全问题的解决方案**



