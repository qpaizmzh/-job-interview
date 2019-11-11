# spring-mybatis整合

* 要有spring，springMVC，mybatis,c3p0,JDBC等常见的使用的包（可以酌情进行更换）
* 配置web.xml
  * ```
    //在配置mapperLocations的时候，发现resources下的文件夹不需要按照包名把点号变成斜杠，
    而在源码包的时候就需要把全包名的点变成斜杠,这里可以看到对比
    <!--指定mapper文件的位置-->
            <property name="mapperLocations" value="classpath:com.SSM.Mappers/*.xml"></property>
    <!--mapperLocations: 指定mapper文件的位置-->
    		<property name="mapperLocations" value="classpath:mybatis/mapper/*.xml"></property>
    ```

```XML
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
  <display-name>MyBatis_06_ssm</display-name>

  <!--Spring配置： needed for ContextLoaderListener -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>

    <!-- Bootstraps the root web application context before servlet initialization -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <!-- SpringMVC配置 -->
    <!-- The front controller of this Spring Web application, responsible for handling all application requests -->
    <servlet>
        <servlet-name>spring</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <!-- Map all requests to the DispatcherServlet for handling -->
    <servlet-mapping>
        <servlet-name>spring</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

</web-app>
```









