# LoadClass和forName的区别

## 类的加载方式：

* 隐式加载：new 
* 显式加载：loadClaas,forName等

## 区别：

* ![](/区别/1.png)
* Class.forName得到的class是已经初始化完成的
* ClassLoader.loadClass得到的class是还没有链接的



