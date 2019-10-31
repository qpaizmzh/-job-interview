# 死锁

* 必要条件

  * 互斥--一个线程占用着一个资源，如果此时还有其他线程想占用该资源，则需要等待线程的释放，才能由其他线程抢占

  * 请求和保持--一个线程占用一个资源的同时又想拿另一个资源，但是另一个资源被其他线程占用，此线程被占用，而且本身自己也有个资源没有释放，导致一直僵持

  * 不剥夺--就是资源必须等待一个线程执行完成之后才可被抢夺

  * 环路等待--就是A需要B，B也需要A，两个线程相互堵塞

  * ```java
    //死锁的例子
    package com.mmall.concurrency.example.deadLock;

    import lombok.extern.slf4j.Slf4j;

    /**
     * 一个简单的死锁类
     * 当DeadLock类的对象flag==1时（td1），先锁定o1,睡眠500毫秒
     * 而td1在睡眠的时候另一个flag==0的对象（td2）线程启动，先锁定o2,睡眠500毫秒
     * td1睡眠结束后需要锁定o2才能继续执行，而此时o2已被td2锁定；
     * td2睡眠结束后需要锁定o1才能继续执行，而此时o1已被td1锁定；
     * td1、td2相互等待，都需要得到对方锁定的资源才能继续执行，从而死锁。
     */

    @Slf4j
    public class DeadLock implements Runnable {
        public int flag = 1;
        //静态对象是类的所有对象共享的
        private static Object o1 = new Object(), o2 = new Object();

        @Override
        public void run() {
            log.info("flag:{}", flag);
            if (flag == 1) {
                synchronized (o1) {
                    try {
                        Thread.sleep(500);
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                    synchronized (o2) {
                        log.info("1");
                    }
                }
            }
            if (flag == 0) {
                synchronized (o2) {
                    try {
                        Thread.sleep(500);
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                    synchronized (o1) {
                        log.info("0");
                    }
                }
            }
        }

        public static void main(String[] args) {
            DeadLock td1 = new DeadLock();
            DeadLock td2 = new DeadLock();
            td1.flag = 1;
            td2.flag = 0;
            //td1,td2都处于可执行状态，但JVM线程调度先执行哪个线程是不确定的。
            //td2的run()可能在td1的run()之前运行
            new Thread(td1).start();
            new Thread(td2).start();
        }
    }

    ```



