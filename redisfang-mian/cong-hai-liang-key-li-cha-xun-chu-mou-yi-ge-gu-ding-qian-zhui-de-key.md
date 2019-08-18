# 从海量Key里查询出某一个固定前缀的Key

## 使用keys对线上业务的影响

* KEY pattern:查找所有符合给定模式pattern的key
  * KEY指令一次性返回所有匹配的key
  * 键的数量过大会使服务卡顿
  * ![](/keys/1.png)



