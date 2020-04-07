# Eureka

# 简介

- eureka是整个微服务中最为基础的一个模块，它分为eureka server和eureka client，server的主要作用是作为一个服务调用的中心，将注册上来的微服务进行统一的管理，由特定的算法比如负载均衡，将特定的请求分发到指定的特定的微服务上执行，保持负载均衡的能力，而eureka client就是注册到server上的微服务的模块

### 创建流程

#### server

- 创建一个新的工程，导入相应的包，然后在启动类上添加注解：

  ```java
  package com.abc.demo1;
  
  import org.springframework.boot.SpringApplication;
  import org.springframework.boot.autoconfigure.SpringBootApplication;
  import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;
  
  @SpringBootApplication
  @EnableEurekaServer //添加这一个注解
  public class Demo1Application {
  
      public static void main(String[] args) {
          SpringApplication.run(Demo1Application.class, args);
      }
  
  }
  ```

- 在属性文件中定义好相应的配置，下面是简单的配置

  ```properties
  #定义微服务的名字，对于后面的调用的标识就是这个
  spring.application.name=server
  #端口号，有时后一个微服务名有多个端口，这样就可以分发给不同的端口进行执行
  server.port=7002
  #连接这个微服务的主要地址，一般用的是IP地址
  eureka.instance.hostname=localhost
  #是否自身进行注册，这个一般都不用
  eureka.client.register-with-eureka=false
  #是否自行检索注册，这个一般也不用
  eureka.client.fetch-registry=false
  #注册微服务的地址，就是eureka server的地址(这个跟后续微服务中心做集群有用)
  eureka.client.service-url.defaultZone=http://localhost:7001/eureka/
  ```

- 然后就可以进行启动了

#### client

- eureka client客户端的创建简单一些，只不过要添加的是eureka-client的包，这要maven中添加了该包之后，甚至连注解到启动类的@EnableDiscoveryClient注解都不需要添加（这是新版本的特性）

- 注意的还是client中的properties:

  ```properties
  server.port=9001
  spring.application.name=client
  #注册的地址
  eureka.client.service-url.defaultZone=http://localhost:7001/eureka/,http://localhost:7002/eureka/
  ```

  

#### 配置多个server节点

- 要配置多server节点，只需要修改一下配置文件，并且启动多个实例就可以，这里实现的是peer1和peer2两个服务器的地址，这里方便模拟，hostname要不一样，另外方便演示，在hosts文件中同时添加127.0.0.1 peer1 和 127.0.0.1 peer2这样的信息

- 再就是分别修改port和hostname，让peer1的注册到peer2,peer2注册到peer1

  ```properties
  #定义微服务的名字，对于后面的调用的标识就是这个
  spring.application.name=server
  #端口号，有时后一个微服务名有多个端口，这样就可以分发给不同的端口进行执行
  server.port=7002
  #连接这个微服务的主要地址，一般用的是IP地址
  eureka.instance.hostname=peer2
  #是否自身进行注册，这个一般都不用
  eureka.client.register-with-eureka=false
  #是否自行检索注册，这个一般也不用
  eureka.client.fetch-registry=false
  #这里注册的微服务的地址是7001，表示这个7002的服务中心会注册到7001的服务器上，同样，peer2的也要注册到peer1的微服务上
  eureka.client.service-url.defaultZone=http://peer1:7001/eureka/
  ```

#### 配置多个client

- 配置多个微服务中心为的是保持多个微服务能正常运行，即使一个微服务中心崩塌也可以到其他的微服务中心继续调用

- 配置多个相对简单，保持微服务的名字一致，更改的就是端口号和注册的微服务地址：

  ```properties
  server.port=9001
  spring.application.name=client
  #注册的地址（多个注册地址，直接使用逗号进行隔开就可以）
  eureka.client.service-url.defaultZone=http://localhost:7001/eureka/,http://localhost:7002/eureka/
  ```

  