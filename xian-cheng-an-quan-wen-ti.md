# 线程安全问题

## 安全发布对象

## 单例模式的线程安全问题

* 普通的单例模式（饿汉模式）

```java
/***
 * 普通的单例模式--非线性安全
 */
public class ConcurrencyTest {
    private ConcurrencyTest() {
    }

    private static ConcurrencyTest concurrencyTest = null;

    public ConcurrencyTest getInstance() {
        if (concurrencyTest == null) {//在多线程的情况下容易出现A线程和B线程同时认为实例为空，导致线程不安全
            concurrencyTest = new ConcurrencyTest();
        }
        return concurrencyTest;
    }
}
```

* 给方法加入同步模式

```java
/***
 * 普通的单例模式--线性安全
 */
public class ConcurrencyTest {
    private ConcurrencyTest() {
    }

    private static ConcurrencyTest concurrencyTest = null;

    //这个可以保证线程安全，但是每一次调用都要添加一个锁，非常的耗性能
    public synchronized ConcurrencyTest getInstance() {
        if (concurrencyTest == null) {
            concurrencyTest = new ConcurrencyTest();
        }
        return concurrencyTest;
    }
}
```

* 懒汉模式

```java
/***
 * 线性安全
 */
public class ConcurrencyTest {
    private ConcurrencyTest() {
    }
    //这样确实可以线程安全，但是一开始从类加载就创建实例，容易浪费资源
    private static ConcurrencyTest concurrencyTest = new ConcurrencyTest();;

    public  ConcurrencyTest getInstance() {

        return concurrencyTest;
    }
}
​
```

* 双重验证

```java
/***
 * 普通的单例模式--线性安全
 */
public class ConcurrencyTest {
    private ConcurrencyTest() {
    }

    private static ConcurrencyTest concurrencyTest = null;

    //这个可以保证线程安全，但是每一次调用都要添加一个锁，非常的耗性能
    public synchronized ConcurrencyTest getInstance() {

        //这里同样没有线程安全
        //CPU创建实例，经过分配内存对象空间，初始化对象，把变量指向分配的内存空间
        //实际运行的时候， 有可能会出现初始化对象和变量分配内存空间调换的情况，导致以为对象已经创建，但是实际上初始化并没有完成的情况
        if (concurrencyTest == null) {
            synchronized (ConcurrencyTest.class) {
                if (concurrencyTest == null) {
                    concurrencyTest = new ConcurrencyTest();
                }
            }
        }

        return concurrencyTest;
    }
}
​
```

* 在里面添加枚举类

```java
/***
 * 普通的单例模式--线性安全
 */
public class ConcurrencyTest {
    private ConcurrencyTest() {
    }
    //enum自带static final属性，不会允许其他的修改，而且在类加载的时候就创建好枚举
    //等待调用的时候才会进行创建，而final关键字也保证了线程安全
    private enum createConcurrencyInstance {
        INSTANCE;

        private ConcurrencyTest singleton;

        createConcurrencyInstance() {
            singleton = new ConcurrencyTest();
        }

        public ConcurrencyTest getInstance() {
            return singleton;
        }
    }

    //
    public ConcurrencyTest getInstance() {
        return createConcurrencyInstance.INSTANCE.getInstance();
    }
}
```

## 堆栈封闭

* 在方法里面的定义的局部变量不受其他线程访问的影响，完全是由单独的线程操作，所以不涉及线程安全的问题

---

## 线程封闭

* 概念就是一个对象的访问只能一个线程访问，相当于让一个对象的访问固定在某一个线程上

* 常用的线程封闭的方法就是直接只用的ThreadLocal就可以：

  * 内部是ThreadLocal有个静态内部类ThreadLocalMap，帮每一个Thread都维护了一个数组table，ThreadLocal确定了一个数组的下标，这个下标就是value存储的对应的位置

  * 常常应用于包装一些业务类的ID，比如userID,事务ID,单独处理业务才需要用到类似的性能，相当于用空间换取时间

---

## 线程不安全的类

* StringBuffer是线程安全，它内部的方法基本上使用了synchronized关键字，性能比较弱，如果是StringBuilder是线程不安全的，但是性能较高，一般情况下都是使用后者，要想保证后者的线程安全，只需要在方法里面新创建一个StringBuilder，让其变成局部变量，便不再有线程安全的问题

* DateTime类比SimpleDateFormate类更好用，而前者还有线程安全，后者没有，还有其他相关前者比后者多的优势，这个自己去了解一下

* ArrayList,HashSet,HashMap，也是线程不安全的类

* 先检查后执行的语句，也是容易出现线程不安全的问题

## 同步容器（不能做到绝对的线程安全）

* 常见的同步容器有：hashTable，vector,Collections.synchornizedXXX\(\)等，但是他们也不能做到最对的线程安全

* 在对这些同步容器中，使用了foreach循环或者迭代器循环的时候，如果出现了增、删等操作的话，会出现并发异常的错误，而for循环的话一般不会出现这种错误



