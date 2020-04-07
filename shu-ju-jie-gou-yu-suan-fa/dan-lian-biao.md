# 单链表

- 单链表的简单的数据结构，可用于无序插入的时候自动把插入的节点进行排序并进行放回（其实完全可以使用SortedMap或者是SortedSet来代替，这里只是感受一下这个实现方式是怎么样的）

- 代码：

  ```java
  public class LinkedListDemo {
  
      public static void main(String[] args) {
          LinkedList linkedList = new LinkedList();
  //        linkedList.add(new Node(1, "ANC"));
  //        linkedList.add(new Node(3, "ACC"));
  //        linkedList.add(new Node(2, "ABC"));
  //        linkedList.add(new Node(4, "ADC"));
  
  
          linkedList.addByOrder(new Node(1, "ANC"));
  
          linkedList.addByOrder(new Node(3, "ACC"));
  
          linkedList.addByOrder(new Node(2, "ABC"));
  
          linkedList.addByOrder(new Node(4, "ADC"));
          linkedList.modify(new Node(4, "ADCD"));
          linkedList.modify(new Node(5, "ADCDE"));
  
          linkedList.del(4);
  
          linkedList.show();
      }
  }
  
  class LinkedList {
      static final Node headNode = new Node();
  
      public void add(Node node) {
          Node temp = headNode;
          while (true) {
              if (temp.next == null) break;
              temp = temp.next;
          }
          temp.next = node;
      }
  
      public void addByOrder(Node node) {
          Node temp = headNode;
          //有其他节点的情况
          while (temp.next != null) {
              //如果找到当前节点的下一个节点的ID比要插入的节点大，说明插入位置就在当前节点的后面
              if (temp.next.id > node.id) {
                  node.next = temp.next;
                  temp.next = node;
                  return;
              }
              if (temp.next.id == node.id) {
                  System.out.println("重复了");
                  return;
              }
              temp = temp.next;
          }
          temp.next = node;
      }
  
      public void modify(Node modifyNode) {
          Node temp = headNode;
          while (temp.next != null) {
              if (temp.next.id == modifyNode.id) {
                  temp.next.name = modifyNode.name;
                  System.out.println("修改成功");
                  return;
              }
              temp = temp.next;
          }
      }
  
      public void del(int id) {
          Node temp = headNode;
          while (temp.next != null) {
              if (temp.next.id == id) {
                  //这里有些难以理解
                  temp.next = temp.next.next;
                  return;
              }
              temp = temp.next;
          }
          System.out.println("找不到要删除的节点");
      }
  
      public void show() {
          if (headNode.next == null) {
              System.out.println("没有节点");
              return;
          }
          Node node = headNode.next;
          while (node != null) {
              System.out.println(node);
              node = node.next;
          }
      }
  
  
  }
  
  class Node {
      public Node() {
      }
  
      public Node(int id, String name) {
          this.id = id;
          this.name = name;
      }
  
  
      int id;
      String name;
      Node next;
  
      public String getName() {
          return name;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  
      public Node getNext() {
          return next;
      }
  
      public void setNext(Node next) {
          this.next = next;
      }
  
      @Override
      public String toString() {
          return "Node{" +
                  "id=" + id +
                  ", name='" + name + '\'' +
                  '}';
      }
  }
  
  ```

  