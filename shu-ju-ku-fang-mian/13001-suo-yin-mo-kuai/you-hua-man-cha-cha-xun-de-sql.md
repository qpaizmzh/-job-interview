### 如何定位并优化慢查询Mysql

具体场景具体分析，只提出大致思路

* 根据慢日志定位慢查询sql
  * 在某个表中使用命令：show variables like %quer%
  * 使用慢日志查询时，关注三个变量：slow\_query\_log slow\_query\_log\_file long\_query\_time，设置某个变量值，使用 set global slow\_query\_log=on等类似的命令
  * 使用命令:show status like '% slow\_queries%'，可以查询慢查询的记录数
* 使用explain等工具分析sql
  * 只需要在查询语句的前面添加explain关键字就可以对查询语句执行进行分析：explain select name from tables;
  * 查询出来字段中重点看的是type和extra，type的类型如果是index或者是all,这个就需要开始进行优化
  * ![](/密集索引和稀疏索引/3.png)
  * extra中出现以下2项说明mysql不能使用索引：
    * Using filesort：表示Mysql会对一些结果使用一个外部索引排序，而不是从表里按索引次序读到相关的内容，可能在内存或者是磁盘进行排序。mysql中无法利用索引完成排序的操作称为：“文件排序“
    * Using temporary:表示mysql在对查询结果排序时使用临时表，常见于排序order by和分组查询group by
  * 测试索引的时候可以使用force index\(索引名\)强制查询语句使用该索引：select × from table force index\(索引名\)来验证各个索引的情况，而添加所以可以使用：alter table tablename add index 索引名（给该字段添加索引的字段名）
* 修改sql或者尽量让sql走索引



联合索引中，mysql的规则是在条件中从左往右进行匹配，如果匹配到了范围查询，像&gt;&lt;between，like等这种符号，终止索引匹配。

![](/优化慢查询/2.png)

![](/密集索引和稀疏索引/5.png)

