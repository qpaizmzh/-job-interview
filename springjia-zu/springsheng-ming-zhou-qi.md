# Bean生命周期

* ![](/bean/1.png)
* ![](/bean/2.png)
* 实现的过程

  * Spring对Bean实例化

  * Spring将值设置到属性上

  * 如果bean实现了BeanNameAware接口，Spring将Bean的Id传递给setBeanName\(\)方法

  * 如果bean实现了BeanFactoryAware接口，Spring将调用setBeanFactory\(\)方法，将BeanFactory容器的实例导入

  * 如果bean实现了ApplicationContextAware接口，Spring将调用setApplicationContext\(\)方法，将bean所在的应用上下文的引用传入进来

  * 如果bean实现了BeanPostProcessor接口，Spring将调用post-ProcessBeforeInitialization\(\)方法

  * 如果bean实现了InitializingBean接口，Spring将调用after-PropertiesSet\(\)方法。类似地，如果bean使用init-method声明了初始化的方法，该方法也会被调用；
  * 如果bean实现了BeanPostProcessor接口，Spring将调用post-ProcessAfterInitialization\(\)方法
  * 此时，bean已经就绪，可以被应用程序使用，直到应用上下文被销毁
  * 如果bean实现了DisposableBean接口，Spring将调用destroy\(\)接口方法。同样，如果bean使用了destroy-method声明了销毁的方法，该方法也会被调用



