# B+TREE

B+树是B树的变体，定义和B树相同，但是：

* 非叶子节点关键字个数和子树指针个数相同
* 非叶子节点的字数和子树指针P\[i\]，指向关键字值在（K\[i\]，K\[i+1\]\)的子树
* 非叶子节点仅用来索引，数据都保存在叶子节点中

B+Tree更适合用来储存索引

* B+树的磁盘读写代价更低，该树除叶子节点外没有直接指向数据库的数据，减少I/O读写次数
* B+树效率稳定，靠的是从根节点到叶子节点间的路径的相同
* B+树更利于数据库的扫描，程序只需要扫描B+树的叶子节点，顺着指针方向就可以找到相应的数据

![](/B+TREE/1.png)

