# 用过的Redis数据类型

## 供用户使用的数据类型

* String，最基本的安全数据类型，二进制安全（值可以存储512M）
* Hash：String元素组成的字典，适用于存储对象，是一个field和value的映射表 常用命令 hmset hset hget
* List列表：按照String元素插入顺序排序  常用命令（lpush lrange）属于后进先出的栈结构 所以可以轻易做出排行榜的功能 能存储41个成员左右
* Set：String元素组成的无序集合，通过哈希表实现，不允许重复 常用指令 sadd smembers
* Sorted Set：通过分数来为集合中的成员进行从小到大的排序
* 用于计数的hyperLogLog,用于支持储存地理位置信息的GEO



