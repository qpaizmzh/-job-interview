# sleep和wait方法的区别

## 基本区别

* sleep是Thread类的方法，wait\(\)是Object的方法
* sleep\(\)方法可以在任何地方使用
  * wait\(\)方法只能在synchronized方法或者是块调用

## 最本质的区别

* sleep方法会把CPU资源释放出来，但是没有权限可以释放锁
* wait方法不但把CPU资源释放，而且也把同步锁进行释放



