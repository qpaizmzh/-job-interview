# Collection体系

## 数据结构考点

* 数组和链表的区别
  * 数组：长度固定，不好扩展，读的性能好，写的性能差
  * 链表：长度不定，容易扩展，读的性能差，写的性能好
* 链表的操作，如反转，链表环路检测，双向链表，循环链表等相关操作
  * 链路检测可以用两个指针，一个快指针（两个两个地跳），一个慢指针，如果是闭环的，一定有一个时间，快指针等于慢指针
* 队列，栈的应用
  * 栈可以设置数的遍历情况
* 二叉树的遍历方式及其递归和非递归的实现
  * 递归实现
    * ```java
      //前序遍历
      public void preOrder(TreeNode t) {
      		if(t != null) {
      			System.out.println(t.val);
      			preOrder(t.left);
      			preOrder(t.right);
      		}
      	}
      //中序遍历	
      public void inOrder(TreeNode t) {
      		if(t != null) {
      			inOrder(t.left);
      			System.out.println(t.val);
      			inOrder(t.right);
      		}
      	}
      //后序遍历
      public void postOrder(TreeNode t) {
      		if(t != null) {
      			postOrder(t.left);
      			postOrder(t.right);
      			System.out.println(t.val);
      		}
      	}
      ```
  * 非递归实现
    * ```java
      //递归调用的本质是使用递归工作栈来进行数据存储, 因此将递归算法转换成非递归算法需要借用栈.
      //前序遍历
      //每向左访问一次节点，上一个节点的右子树就会被遗弃，因此需要用栈来存储被忘记的右子树
      public void preOrder1(TreeNode t) {
      		TreeNode p = t;     //定义一个用来遍历的指针
      		Stack<TreeNode> s = new Stack<>();
      		do {
      			while(p != null) {
      				System.out.println(p.val);
      				if(p.right != null)
      					s.push(p.right);
      				p = p.left;
      			}
      			p = s.pop();
      		}while(!s.empty());
      	}
	
      //中序遍历
      //中序遍历是先一直向左遍历，直到某个节点的左子树为null,才返回访问该节点，但是一路向左遍历的时候没有记下父节点的名字，
      //同样需要栈来存储每次走过的节点
      public void inOrder1(TreeNode t) {
      		TreeNode p = t;     //定义一个用来遍历的指针
      		Stack<TreeNode> s = new Stack<>();
      		while(p != null || !s.empty()){
      			if(p != null){
      				s.push(p);
      				p = p.left;
      			}
      			else{
      				p = s.pop();
      				System.out.println(p.val);
      				p = p.right;
      			}
      		}
      	}

      //后序遍历
      //后续遍历是访问左右节点之后才能访问根节点
      //每次操作的节点都可以看做根节点，访问根节点的时候需要知道右子节点是否被访问过
      //引入辅助指针R指向最近一次访问的节点
      //(这块不理解的可以把r指针和有关r的判断语句去掉根据实例走一遍程序就能发现漏洞: 判断节点是否存在右子树的时候会重复访问右节点)
      	public void postOrder1(TreeNode t) {
      		TreeNode p = t;     //定义一个用来遍历的指针
      		TreeNode r = null;  //辅助指针r指向最近一次访问过的节点
      		Stack<TreeNode> s = new Stack<>();
      		do {
      			while(p != null) {
      				s.push(p);
      				p = p.left;
      			}
      			p = s.peek();  //右节点还未访问, 所以此时只是瞄一眼, 还不能取出栈顶元素
      			if(p.right != null && p.right != r) {
      				p = p.right;
      			}
      			else {        //不存在右节点或者右节点已被访问时 才能访问根节点
      				p = s.pop();
      				System.out.println(p.val);
      				r = p;
      				p = null;
      			}
      		}while(!s.empty());
      	}

      //层次遍历
      	public void levelOrder(TreeNode t) {
      		if(t == null) return;
      		TreeNode p = t;
      		LinkedList<TreeNode> queue = new LinkedList<>(); 
      		queue.offer(p);
      		while(!queue.isEmpty()) {
      			p = queue.poll();
      			System.out.println(p.val);
      			if(p.left != null)
      				queue.offer(p.left);
      			if(p.right != null)
      				queue.offer(p.right);
      		}
      	}
      ```

* 红黑树的旋转

## 算法考点

* 内部排序：如递归排序，交换排序（冒泡、快排），选择排序，插入排序；
* 外部排序：应掌握如何利用有限的内存配合海量的外部存储来处理超大的数据集，写不出来也要有相关的思路
* 考点扩展：
  * 哪些排序是不稳定的，稳定意味着什么
  * 不同数据集，各种排序最好或最差的情况
  * 如何优化算法

## 集合List和Set

![](/体系/1.png)

