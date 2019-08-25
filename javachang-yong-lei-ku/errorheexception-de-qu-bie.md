# Error和Exception的区别

## Java异常体系

![](/错误/1.png)

### 从责任角度：

* Error属于JVM需要负担的责任
* RuntimeException是程序应该负担的责任
* Checked Exception可检查异常是Java编译器应该负担的责任

### ![](/错误/2.png)Java异常处理的原则

* ### 具体明确：抛出的异常应能通过异常类名和message准确说明异常的类型和产生异常的原因
* ### 提早抛出：应尽可能早的发现并抛出异常，便于精确定位问题
* ### 延迟捕获：异常的捕获和处理应尽可能延迟，让掌握更多信息的作用域来处理异常


