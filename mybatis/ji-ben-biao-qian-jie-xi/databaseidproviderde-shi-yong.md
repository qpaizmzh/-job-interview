# dataBaseIdProvider的使用

* 这个东西可以使同样的调用方法根据实际的情况调用不同的数据库，自由的切换
* 步骤：
  * 先在全局配置文件创建databaseIdProvider标签，根据不同的数据库创建不同的别名
    ```XML
    DB_VENDOR 对应的 databaseIdProvider 实现会将 databaseId 设置为 DatabaseMetaData#getDatabaseProductName() 
    返回的字符串。 由于通常情况下这些字符串都非常长而且相同产品的不同版本会返回不同的值，
    所以你可能想通过设置属性别名来使其变短，如下：
    <databaseIdProvider type="DB_VENDOR">
      <property name="SQL Server" value="sqlserver"/>
      <property name="DB2" value="db2"/>
      <property name="Oracle" value="oracle" />
    </databaseIdProvider>
    //在提供了属性别名时，DB_VENDOR 的 databaseIdProvider 实现会将 databaseId 设置为第一个数据库产品名与属性中的名称相匹配的
    值，如果没有匹配的属性将会设置为 “null”。 在这个例子中，
    如果 getDatabaseProductName() 返回“Oracle (DataDirect)”，databaseId 将被设置为“oracle”。
    ```
  * 在mapperXML文件中中的mappers标签设置databaseId,值就是databaseIDProvider里面的property的value值



