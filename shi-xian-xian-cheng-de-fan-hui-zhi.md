# 实现处理线程的返回值

* ## 主线程等待

  * \`\`\`java  
    package com.jesus;

    public class Cycle implements Runnable {  
        private String value;

        @Override
        public void run() {
            try {
                Thread.currentThread().sleep(5000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            value="1";
        }

        public static void main(String[] args) {
            Cycle cycle = new Cycle();
            Thread th = new Thread(cycle);
            th.start();
            System.out.println("value:"+cycle.value);
        }
    }

    ```

* ## 使用线程的join方法堵塞调用线程，待运行完成后再进行恢复

```
package com.jesus;

public class Cycle implements Runnable {
    private String value;


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

* 在JDK1.5之后使用Callable&lt;T&gt;和FutureTask&lt;T&gt;来获得子线程的返回值，而且控制的精度要比上两个好，也不用直接堵塞调用线程：
* ```
  package com.jesus;

  import java.util.concurrent.Callable;

  public class MyCallable implements Callable<String> {
      @Override
      public String call() throws Exception {
          String value = "123";
          System.out.println("start");
          Thread.currentThread().sleep(5000);
          System.out.println("task done");
          return value;
      }
  }


  package com.jesus;

  import java.util.concurrent.ExecutionException;
  import java.util.concurrent.FutureTask;

  public class MyFutureTask {
      public static void main(String[] args) throws ExecutionException, InterruptedException {
          FutureTask<String> task = new FutureTask<String>(new MyCallable());
          new Thread(task).start();
          if (!task.isDone()){
              System.out.println("not finished");
          }
          System.out.println("return:"+task.get());
      }
  }

  ```



