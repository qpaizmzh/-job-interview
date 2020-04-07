# SpringSecurity

### 使用SpringSecurity的基本方法

- 因为SpringSecurity是要给Servlet中的Filter接口添加拦截功能，在定义中直接使用了FilterChainProxy作为接口来实现，在Spring中一般提供的是SecurityConfigurer接口，通过它可以实现SS（SpringSecurity缩写）的配置，但很多东西都是不需要额外配置的，所以直接SS还出了一个抽象类WebSecurityConfigurerAdapter，所以自定义的时候，**直接继承这个类并重写configure方法就可以**
- 把具体的配置放到前面，把不具体的配置放到后面

### 强制使用HTTPS

- Spring中可以强制使用https的请求，在重写configure方法时，添加requiresChannel方法，使用方法匹配地址之后，使用requireSeure()方法，就可以强制使用https方法，而取消https请求的使用requireInSecure()方法：

  ```java
  http.requiresChannel().antMatchers("/**").requiresSecure();//强制https请求
  http.requiresChannel().antMatchers("/**").requiresInsecure();//取消https请求
  ```

  

