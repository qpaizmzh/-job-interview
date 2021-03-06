# HTTP相关

## http简介

* http是一个集请求和响应、无状态的应用层协议，全名叫做超文本传输协议
* 支持客户/服务器模式（B/S）
* 简单快速，客户端连接服务器只需要传输请求方法和路径就可以连接，并且HTTP协议简单，服务器的HTTP程序较小，通信速度较快
* 灵活：HTTP允许传输任意类型的对象，但是需要在Content-type中声明传输的类型
* 无连接：在客户端发送并且受到响应之后就断开连接，每个连接只处理一个请求，节省传输时间，但是HTTPS会有keepAlive模式，保持一段时间后断开
* 无状态：对事务处理没有记忆能力，如果这次的连接需要上一次连接处理的事务状态的话，需要进行重传，可能导致数据量传输增大

## HTTP报文结构

![](/HTTP相关/1.png)

## ![](/HTTP相关/2.png)

## HTTP请求/相应的步骤

* 客户端连接到web服务器（建立一个TCP套嵌字连接）

* 发送HTTP请求

* 服务器接受请求并返回HTTP相应（服务器根据请求定位请求资源，解析之后写入到TCP套嵌字，然后发送给客户端）

* 释放TCP连接

## 在浏览器地址栏输入URL之后，按下回车之后经历的流程

* DNS解析：浏览器会依照URL逐层查询DNS缓存，解析URL中的域名对应的IP地址
* TCP连接
* 发送HTTP请求
* 服务器处理请求并返回请求报文
* 浏览器解析渲染页面
* 连接结束

## HTTP状态码

* 1XX：只是信息--表示请求已接收，继续处理
* 2XX：表示信息已接受、成功解析
* 3XX:重定向--要完成请求必须进行更进一步的操作
* 4XX：客户端错误--请求有语法错误或者请求无法实现
* 5XX：服务端错误--服务器未能实现合法的请求
* 常见状态码：
  ![](/HTTP相关/3.png)

## GET请求和POST请求的区别

* HTTP报文层面：GET请求将信息放在URL，POST放在报文体中，目前只能说POST安全性比GET请求好一点，但其实想读的话还是可以通过抓包读取请求报文体的
* 数据库层面：GET符合幂等性和安全性，POST不符合。幂等性是指对数据库的一次操作和多次操作的结果是一致的，安全性指的是对数据库的操作没有改变数据库中的数据。因为GET请求使用于查询用的，所以GET请求不会改变数据库中的任何数据，可以大致的认为只是符合幂等性和安全性的，而post请求是基于上一个URL请求作用的，每一次的请求都会叠加新的资源，这也是POST和PUT请求的最大区别，PUT是幂等的
* 其他层面：GET可以被缓存、储存，在浏览器记录就有体现，POST不行

## Cookie和Session的区别

### Cookie简介

* 是由服务器发给客户端的特殊消息，服务器会把该消息放到Response Header中，客户端接收到之后以文本的形式保存在客户端中
* 客户端再次请求的时候，会把对应的Cookie信息放到请求头中（request header）
* 服务器接收到之后，解析Cookie生成的与客户端相对应的内容，例子就像比如不需要二次登录

### Cookie的设置以及发送过程

![](/HTTP相关/4.png)

### Session简介

- 服务器端的机制，在服务器上保存信息（使用类似散列表的形式保存）
- 解析客户端请求并且操作session id,按需保存状态信息

### Session的实现方式

- 使用Cookie实现
- 在URL回写实现（URL回写是指服务器在给浏览器页面的所有连接中都携带JSessionID的参数，这样客户端点击任意连接都会把Jessionid带回服务器），直接在地址栏输入该资源的ID是匹配不到服务器对应的资源的，Tomcat会值允许这两种实现方式的一种，然后禁止另一种

### Cookie和Session的区别

- Cookie数据存放在客户的浏览器上，Seesion数据放在服务器上
- Session相对于Cookie安全
- 考虑减轻服务器负担，应当使用Cookie

### SSL（Security Sockets layers 安全套接层）

- 为网络通信提供安全及数据完整性的一种安全协议
- SSL位于应用层和TCP之间，是操作系统对外API，SSL3.0更名为TLS
- 采用身份认证和数据加密保证网络安全

### 加密的方式

- 对称加密：加密和解密使用同一个密钥
- 非对称加密：加密和解密用的密钥是不同的
- 哈希算法：常见MD5算法，是一种不可逆的将信息转换成任意长度字符的算法
- 数字签名：证明某个消息或者文件是某人发出的，一般会带有Hash值，并且将该值加密，以防被篡改

### https数据传输的流程

- 浏览器将支持的加密算法信息发送给服务器
- 服务器选择一套浏览器支持的加密算法，以证书的形式返回给浏览器
- 浏览器验证证书的合法性，验证成功后，浏览器会随机生成一串密码，并使用证书中的公钥进行加密，之后就是使用根据上面约定好的加密算法，生成随机数，对消息进行加密，将消息回发服务器
- 服务器接收到消息后，使用私钥解密，确定密码，通过密码解密接收到的浏览器发送过来的消息，验证hash码是否和浏览器一致，之后服务器会使用公钥加密新的握手信息发送给浏览器
- 浏览器解密解析对应的握手消息，如果和服务器发送过来的HASH值一致，则此握手过程结束后，服务器和浏览器会使用浏览器之前生成的随机密码以及对称密钥算法进行加密，然后交换数据

### HTTPS和HTTP的区别

- HTTPS需要申请CA证书，而HTTP的不需要
- HTTPS密文传输，HTTP明文传输
- 连接方式不同，HTTPS默认443端口，http默认80端口
- HTTPS=HTTP+加密+认证+完整性保护，比HTTP安全
- HTTPS不是绝对安全，因为某些浏览器默认填充http://，请求需要跳转，存在被劫持的风险，这时可以使用HSTS（HTTP Strict Transport Security）来优化

