# 数组模拟循环队列

## 关键思想

- 头指针和尾指针的设置，头指针包含第一个元素，而尾指针指向最后一个元素的后一个位置
- 实际的存储的元素的个数是设置的队列的元素的n-1个，要留一个位置给尾指针存放
-  判断是否为空，同样是判断头指针和尾指针是否处于同一个位置，而判断是否队列满则是验证尾指针的下一个指针是否是头指针的位置，这个计算需要按模取值

### 代码：

```java
package org.javaboy.vhr;

public class ArrayQueueDemo {

    //要提高数组模拟队列的利用率，需要做成循环数组，并且在计算前指针和后指针的地方要进行修改

    private int front = 0; //前部的指针，指向最前的数据
    private int rear = 0;//尾部的指针,同样是指向数据的地方
    private int[] array;
    private int size;

    //数组模拟队列的关键是流出一个空格存储尾指针，避免两个指针重合的时候无法判断是否满还是空，所以一个size的元素实际上只能存储size-1个元素
    public ArrayQueueDemo(int size) {
        array = new int[size];
    }

    public boolean isFull() {
        //尾索引的下一个索引是头索引的话就是队列满的时候，和单纯的头指针等于尾指针不同，归根结底是环形队列
        return front == (rear + 1) % size;
    }

    public boolean isEmpty() {
        //如果front指针和rear指针重叠，说明这个队列已经为空
        return front == rear;

    }

    public void add(int number) {
        if (isFull()) {
            System.out.println("队列已经满");
        } else {
            array[rear] = number;
            rear = (rear + 1) % size;
            System.out.println("添加成功");
        }
    }

    public int pop() {
        if (isEmpty()) {
            System.out.println("队列为空，不能再弹出节点");
            return -1;
        } else {
            //front指针往后移
            int temp = array[front];
            front = (front + 1) % size;
            return temp;
        }
    }

    public void show() {
        if (!isEmpty()) {
            for (int i = front; i < front + (rear + size - front) % size; i++) {
                System.out.printf("第%d个：%d", i % size, array[i % size]);
            }
        }
    }

}

```

