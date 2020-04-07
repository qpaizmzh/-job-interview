# Zuul网关

- 自动使用负载均衡
- 保护真正的IP地址，避免直接攻击服务器

## 使用

- 新建工程，添加注解@EnableZuulProxy，这个注解自动包含熔断机制

- 添加属性，让其注册到微服务中心上

- 启动之后，比如要访问上面的任意微服务的方法，就可以这样使用：http://{zuul网关地址}:{port}/{微服务名字}/{微服务控制器的登录名}，这样有效隐藏真正服务器的IP地址，减少被攻击的危险

- 除此以外，zuul网关还有另外的配置匹配URL的方式，因为一个微服务有可能好几个端口，所以一般使用面相微服务的映射规则：

  ![image-20200322164033880](D:\software\programming\笔记\面试\-job-interview\image-20200322164033880.png)

## 使用zuul过滤器

- 因为有可能在使用网关的时候会有权限检查的方式，所以可能会有相关的过滤需要实现，一般直接继承ZuulFilter这个抽象类，并实现相应的方法就可以，这里需要redis进行辅助

![image-20200322164612269](D:\software\programming\笔记\面试\-job-interview\image-20200322164612269.png)

![image-20200322164724115](D:\software\programming\笔记\面试\-job-interview\image-20200322164724115.png)

![image-20200322164758693](D:\software\programming\笔记\面试\-job-interview\image-20200322164758693.png)

![image-20200322164833969](D:\software\programming\笔记\面试\-job-interview\image-20200322164833969.png)