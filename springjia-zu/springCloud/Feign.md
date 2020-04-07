# Feign声明式的调用

## 简介

- fegin的是声明式的调用，通过直接注解的方式调用某个项目接口的上的方法来直接调用，省去了像Ribbon中的多次编写URL和端口来对特定端口进行调用的麻烦

## 简单使用

- 在客户端的启动类上添加@EnableFeginClients注解，启动feign

  ```java
  @SpringBootApplication
  //这里不再需要添加eurekaClient的注解，只要包含了eurekaClient的模块就可以进行使用
  @EnableFeignClients
  public class ClientdemoApplication {
  
      //初始化RestTemplate（这个是使用ribbon才会用到的，feign一般用不到）
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

- 定义要调用的接口，指定的调用哪个微服务中的哪个控制器的方法，和实际定义的微服务的方法声明保持一致

  ```java
  //指定微服务的ID
  @FeignClient("user")
  public interface UserService {
      //指定通过HTTP的方法来获得请求的路径
      @GetMapping("/user/{id}")
      UserPo getUser(@PathVariable("id") Long id);
  
      @PostMapping("/insert")
      Map<String, Object> insert(@RequestBody UserPo po);
  
      @PostMapping("/update/{userName}")
      public Map<String, Object> update(@PathVariable("userName") String userName,
                                        @RequestHeader("id") Long id);
  
      @GetMapping("/timeout")
      public String timeout();
  }
  ```

- 在控制器中调用被feign注解的Service的方法

  ```java
  @RestController
  public class ProductController {
      @Autowired
      @Qualifier("restTemplate")
      private RestTemplate restTemplate;
  
      //加入了feign之后的调用方式
      @Autowired
      private UserService userService;
  
      //这是ribbon的调用方法，因为要经常写getForObject这个方法，不太优雅，所以一般换成feign进行调用
      @GetMapping("/ribbon")
      public UserPo testribbon() {
          UserPo userPo = null;
  //        for (int i = 0; i < 10; i++) {
  //            userPo = restTemplate.getForObject("http://user/user/" + (i + 1), UserPo.class);
  //        }
  
          //使用feign客户端的方式（自带负载均衡）
          for (int i = 0; i < 10; i++) {
              userPo = userService.getUser((long) (i + 1));
          }
          return userPo;
      }
  
  
      @GetMapping("/feign2")
      public Map<String, Object> testribbon2() {
          Map<String, Object> result = null;
          UserPo userPo = null;
  
          //使用feign客户端的方式
          for (int i = 0; i < 10; i++) {
              Long id = (long) i;
              userPo = new UserPo();
              userPo.setId(id);
              userPo.setLevel((int) (id % 3 + 1));
              userPo.setUsername("user_name" + id);
              userPo.setNote("note_" + id);
              result = userService.insert(userPo);
          }
          return result;
      }
  
  
      @GetMapping("/feign3")
      public Map<String, Object> testribbon3() {
          Map<String, Object> result = null;
  
          //使用feign客户端的方式
          for (int i = 0; i < 10; i++) {
              Long id = (long) i;
              String userName = "userName_" + id;
              result = userService.update(userName, id);
          }
          return result;
      }
  
  ```

  