# nginx负载均衡和集群

## 负载均衡

### 四层负载均衡

- F5硬负载均衡
- LVS四层负载均衡
- Haproxy四层负载均衡
- Nginx四层负载均衡

### 七层负载均衡

- Nginx七层负载均衡
- Haproxy七层负载均衡
- apache七层负载均衡

### DNS地域负载均衡

![image-20200407163220245](..\image-20200407163220245.png)

## Nginx简单集群的配置

- 看代码：

  ```
  #代码块没有分号,填写属性用空格隔开，语句结束使用分号，检查配置文件语法可以到sbin文件夹得nginx目录，使用‘-t’命令就可以
  #配置上游服务器
  upstream tomcats {
  	server  192.168.2.123:8080;
  	server  192.168.2.124:8080;
  	server  192.168.2.125:8080;
  }
  
  server {
  	listen  80; #80端口默认可以省略端口号
  	server_name  www.tomcats.com; #这个是域名
  	
  	location / { //映射的路径是'/' #表示任意路径
  		proxy_pass http://tomcats; #http协议开头是要写的，这里的tomcats对应的是upstream的哪个名称
  	}
  }
  ```

- 到sbin文件夹的nginx文件，使用-s reload命令重启：../sbin/nginx -s reload