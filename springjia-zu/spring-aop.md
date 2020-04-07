# Spring AOP

Aspect Oriented Programming

* 切面（ASpect）：Aspect声明类似于Java类中的声明，在Aspect中会包含一些PointCut以及相应的Advice
* 连接点\(Join Point\)：表示要AOP要切入那些地方的点，包括想切入的类，方法等，自身还可以嵌套其他的join Point
* 切点（pointcut）：表示一组 joint point，这些 joint point 或是通过逻辑关系组合起来，或是通过通配、正则表达式等方式集中起来，它定义了相应的 Advice 将要发生的地方。
* 增强\(Advice\):Advice定义了Pointcut里面定义的程序具体要做的操作，它通过before,after和around来区别是在每个 joint point 之前、之后还是代替执行的代码。
* Target（目标对象）：织入 Advice 的目标对象.。
* Weaving（织入）：将Aspect和其他的对象连接起来，并创建Advice object的过程
* 大致关系就是：Joint Point 就是一堆准备切入的方法的集合，pointcut就是切入的规则，用来筛选合适的Joint Point，然后Advice就是实施的方式，比如是在之前还是之后执行还是异常执行等等

### Advice的类型

* before advice, 在 join point 前被执行的 advice. 虽然 before advice 是在 join point 前被执行, 但是它并不能够阻止 join point 的执行, 除非发生了异常\(即我们在 before advice 代码中, 不能人为地决定是否继续执行 join point 中的代码\)
* after return advice, 在一个 join point 正常返回后执行的 advice

* after throwing advice, 当一个 join point 抛出异常后执行的 advice

* around advice, 在 join point 前和 joint point 退出后都执行的 advice. 这个是最常用的 advice.

* introduction，introduction可以为原有的对象增加新的属性和方法

### Spring AOP原理\(说的是Java的动态代理机制\)

* 此时的SpringAOP框架在某种程度上扮演着一个上帝的角色：

  **它知道你在这个框架内所做的任何操作，你对每一个实例对象的非final的public方法调用都可以被框架察觉到！**

* InvocationHandler的作用是将代理类中调用的真实方法的之前、业务代码调用、之后或者其他的操作交由这个接口进行实现，而不是直接由动态代理类包裹所有的之前、业务代码和之后的操作

* 代理模式中需要Proxy和realSubject共同实现相同的功能，两种方式：一个是功能接口，另一个就是继承，让Proxy继承Subject，在获取同样功能的同时，也能通过重写实现多态

* 在代理机制中的继承方式，实现 MethodInterceptor接口，用来处理对代理类上所有方法的请求（这个接口和JDK动态代理InvocationHandler的功能和角色是一样的）

* 在代理机制的接口方式中，做法是先确定要代理的类的实现的所有接口，然后创建对应的classLoader,接口的数量，invocationHandler的实现类，交由Proxy，创建动态的代理类

  * 动态的代理类同样会生成和实际调用类中一模一样的接口对应的方法，只是里面的实际调用的方法全部交由InvocationHandler进行操作，而代理类会在静态模块中自动生成所有public方法中Method对象，作为参数供由InvocationHandler进行实际方法的调用



