# IOC原理

* IOC（Inversion of Control）
  * Spring Core最核心的部分
  * 先了解依赖注入（Dependency Inversion）
    * 上层建筑依赖下层建筑的情况（弊端就是每一次改底层的tire代码，都需要重新对上层的代码进行修改）：![](/IOC/1.png)
    * 依赖注入，相当于是把底层类的实例作为参数直接传递给上层类，实现上层对下层的控制“控制”![](/IOC/2.png)依赖注入的核心方式除了构造器，还有setter注入，接口注入，本质上都是上层的类实现控制反转



