# 实现处理线程的返回值

```java
  @Override
  public void run() {
      try {
          Thread.currentThread().sleep(5000);
      } catch (InterruptedException e) {
          e.printStackTrace();
      }
      value="1";
  }

  public static void main(String[] args) throws InterruptedException {
      Cycle cycle = new Cycle();
      Thread th = new Thread(cycle);
      th.start();
      th.join();//比原来的方法添加了一个堵塞调用线程的方法，需等线程结束之后才能进行其他的操作
      System.out.println("value:"+cycle.value);
  }
}
```

* ## 在JDK1.5之后使用Callable和FutureTask来获得子线程的返回值，而且控制的精度要比上两个好，也不用直接堵塞调用线程：
* ```java
  package com.jesus;
  importjava.util.concurrent.Callable;
  public class MyCallable implements Callable {
  @Override
  public String call() throws Exception {
  String value = "123";
  System.out.println("start");
  Thread.currentThread().sleep(5000);
  System.out.println("task done");
  return value;
  }
  }
  packagecom.jesus;
  import java.util.concurrent.ExecutionException;
  import java.util.concurrent.FutureTask;
  public class MyFutureTask {
  public static void main(String[] args) throws ExecutionException, InterruptedException {
  FutureTask task = new FutureTask(new MyCallable());
  new Thread(task).start();
  if (!task.isDone()){
  System.out.println("not finished");
  }
  System.out.println("return:"+task.get());
  }
  }
  ```
* ## 使用线程池的方法同样可以达到相应的效果
* ```java
  package com.jesus;

  import java.util.concurrent.*;

  public class MyFutureTask {
      public static void main(String[] args){
          ExecutorService service = Executors.newCachedThreadPool();
          Future<String> submit =
                  service.submit(new MyCallable());
          if (!submit.isDone()){
              System.out.println("no finished");
          }
          try {
              System.out.println(submit.get());
          } catch (InterruptedException e) {
              e.printStackTrace();
          } catch (ExecutionException e) {
              e.printStackTrace();
          }finally {
              service.shutdown();//线程池使用完成记得关闭
          }
      }
  }

  ```



