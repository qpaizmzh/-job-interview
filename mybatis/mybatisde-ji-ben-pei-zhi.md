# mybatis的基本配置

* mybatis的XML配置如下，在Springboot上使用的yml配置可以用作适当的参考

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql:///jpa"/>
                <property name="username" value="root"/>
                <property name="password" value="085436"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
//这里添加想要放置的mapper.xml
        <mapper resource="DepartmentMapper.xml"/>
    </mappers>
</configuration>
```

* 然后可以开始放置基本的Mapper文件：

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.mybatis.dao.DepartmentDAO">
    <select id="getById" resultType="com.mybatis.entities.Department">
        SELECT * from departmentSSP where id=#{id}
    </select>
</mapper>
```



