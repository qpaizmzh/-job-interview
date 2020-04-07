# Hystrix熔断器

- 为了体验更好，微服务间的调用出现问题的时候会自动调用配置好的熔断器的方法，减少用户的等待

## 使用

- 在启动类中，添加@EnableCircuitBreaker这个注解

  ```JAVA
  package com.cloud.clientdemo;
  
  import org.springframework.boot.SpringApplication;
  import org.springframework.boot.autoconfigure.SpringBootApplication;
  import org.springframework.cloud.client.circuitbreaker.EnableCircuitBreaker;
  import org.springframework.cloud.client.loadbalancer.LoadBalanced;
  import org.springframework.cloud.openfeign.EnableFeignClients;
  import org.springframework.context.annotation.Bean;
  import org.springframework.web.client.RestTemplate;
  
  @SpringBootApplication
  //这里不再需要添加eurekaClient的注解，只要包含了eurekaClient的模块就可以进行使用
  @EnableFeignClients
  @EnableCircuitBreaker//和熔断器有关的注解
  public class ClientdemoApplication {
  
      //初始化RestTemplate
     // @LoadBalanced
      @Bean(name = "restTemplate")
      public RestTemplate initRestTemplate() {
          return new RestTemplate();
      }
  
      public static void main(String[] args) {
  
          SpringApplication.run(ClientdemoApplication.class, args);
  
      }
  
  }
  
  ```

- 给要添加熔断器的控制器的方法添加相应的注解,值是添加同样返回类型的方法的名字：

  ```java
     @GetMapping("/cir1")
  	//另外这个注解可用的属性比较多，包括自定义时间超过多少来调用error方法等
      @HystrixCommand(fallbackMethod = "error")//对应的是方法名字，指定降级的方法
      public String cir1() {
          return userService.timeout();
      }
   //错误返回的方法
      public String error() {
          return "超时出错";
      }
  ```

## Hystrix仪表盘

- 添加仪表盘，需要在启动类中添加@HystrixDashborad注解（要独立于eureka client，新建一个工程）

  ```java
  
  @SpringBootApplication
  @EnableHystrixDashboard
  public class DashboardApplication {
  
      public static void main(String[] args) {
          SpringApplication.run(DashboardApplication.class, args);
      }
  
  }
  ```

- 给该工程的配置文件简单设置一些参数：

  ```properties
  spring.application.name=hystrix_dashboard
  server.port=6001
  ```

- 然后启动，登录localhost:6001/hystrix，就可以看到仪表盘的界面,这里简单讲解单点监控的操作

  - 给要监控的微服务模块的属性文件添加属性：

    ```properties
    #代表Actuator监控对外暴露的点，默认情况下只会暴露health和info，这里增加hystrix.stream,这样dashboard就可以监控这里的信息
    management.endpoints.web.exposure.include=health,info,hystrix.stream
    ```

- 在仪表盘界面的连接中添加http://localhost:9001/actuator/hystrix.stream这样的连接，设置延迟的时间和名称，点击最下面的按钮就可以开始进行监控

