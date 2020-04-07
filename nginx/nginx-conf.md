# Nginx配置文件详解

- user nobody； 默认在系统中操作的角色权限，可以设置成user root；为最高权限
- work_processes  2; 说明这个worker进程有多少个
- error_log  logs/error.log  info; 配置错误日志文件，后面跟上路径和日志级别
- include imooc.conf; 可以外置配置文件，并在nginx.conf中输入，提高配置文件的可读性
- ![nginx配置文件](..\nginx.conf 核心配置文件.png)

## 重新启动时nginx.pid丢失导致启动nginx的问题

- 有时候重新启动nginx的时候会提示pid文件不见的情况，解决办法是：

  - 按照错误的提示，创建对应的文件夹

  - 找到sbin文件夹，使用命令：./nginx -c /usr/local/nginx/conf/nginx.conf  让其初始化pid文件

    然后再进行重启：./nginx -s reload

### 常用nginx命令

- ./nginx -s quit 正常退出，会等待活动结束后才会退出，如果是stop命令会强制终止，一般不用
- ./nginx -t 检查配置文件语法是否正确
- ./nginx - v 查看版本
- ./nginx -？ 查看帮助文档
- ./nginx  -c 用于指定配置文件路径

### 配置静态文件到nginx的方法

- 找到一个文件夹的地方，例如/home，把相应的静态文件（前端项目）放到该路径下

- 找到nginx文件夹，找到nginx.conf，或者是自己单独新配置导入到nginx.conf的文件，添加相应的路径映射，代码基本如下：

  ```
  server {
  		listen 90;
  		server_name	localhost;
  		
  		location / {
  					root  /home/foodie-shop   #这里的home就是放静态文件的地方，foodie-shop是这个前端项目的文件夹的名字
  					index  index.html
  		}
  		# 下面两种访问的服务器的路径依旧是/home/imooc
  		location /imooc {
  					root  /home  # 这里会访问imooc文件夹里面的资源
  		}
  		location /home/imooc {
  							alias: /static
  		}
  
  }
  ```

  ### 启用gzip压缩静态文件的方法

- 找到nginx.conf，或者自定义的配置文件，然后进行添加：

  ![image-20200407232724005](..\image-20200407232724005.png)

### nginx.conf中的常用location匹配规则

![image-20200407234057279](..\image-20200407234057279.png)