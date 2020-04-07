# 线程池

new Thread的弊端

* * 每次新创建Trhead对象，性能差

  * 线程缺乏统一的管理，可能无限制新建进程，相互竞争资源，有可能占用过多的资源导致死机或者堆内存溢出（OOM）

  * 缺少必要的高级功能，比如更多的执行、定期的执行、线程的中断
* 线程池的好处

  * 重用存在的线程，减少对象创建、消亡的开销，性能佳

  * 可有效控制最大并发线程数，提高系统资源利用率，同时可以避免过多的资源竞争，避免堵塞

  * 提供定时的执行，定期执行，单线程，并发控制线程等等

* ThreadPoolExecutors

  * execute\(\) 提交任务给线程池执行，而submit是提交Callable接口的任务给线程池执行，有返回值

  * shutdown是关闭线程池，等待任务执行完才关闭

  * shutdownNow\(\)是不等待任务执行完立马关闭

  * 监控常用方法：

  ![](file://C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20191028104142997.png?lastModify=1572543488 "image-20191028104142997")

* ![](file://C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20191028104156011.png?lastModify=1572543488 "image-20191028104156011")

* newCachedThreadPool:可以回收一些可用线程池，没有就等待

* newFixedThreadPool:控制最大并发数，超出就等待

* newSheduleThreadPool:可以支持定期任务执行

* newSingleThreadPool：单线程的线程池，保证任务顺序执行

* 代码例子：

  * ```java
        //其他的就不演示了
    //单线程池任务的例子（保证任务按顺序执行）
    ExecutorService executorService = Executors.newSingleThreadExecutor();

            for (int i = 0; i < 10; i++) {
                final int index = i;
                executorService.execute(new Runnable() {
                    @Override
                    public void run() {
                        log.info("task:{}", index);
                    }
                });
            }
            executorService.shutdown();

    //定期任务执行线程池的例子
           ScheduledExecutorService executorService = Executors.newScheduledThreadPool(1);

    //        executorService.schedule(new Runnable() {
    //            @Override
    //            public void run() {
    //                log.warn("schedule run");
    //            }
    //        }, 3, TimeUnit.SECONDS);

            executorService.scheduleAtFixedRate(new Runnable() {
                @Override
                public void run() {
                    log.warn("schedule run");
                }
            }, 1, 3, TimeUnit.SECONDS);
    //        executorService.shutdown();

            Timer timer = new Timer();
            timer.schedule(new TimerTask() {
                @Override
                public void run() {
                    log.warn("timer run");
                }
            }, new Date(), 5 * 1000);
        }
    ```

* 合理配置

  * CPU密集任务，参考值可以设置成NCPU+1

  * IO密集任务，参考值可以设置成2\*NPUC



