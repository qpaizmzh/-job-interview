# 检索文件内容

## grep

* 语法：grep \[options\] pattern file
* 全称：Global Regular Expression Print
* 作用：查找文件符合条件的字符串
* 例子：grep "abc" abc\* ，其中“abc”是查找目标匹配的字符串，而abc\*匹配的是包含该字符串的文件名

### 管道操作符

* 将指令连接起来，前一个指令的输出作为后一个指令的输入

#### 使用管道注意的点

* 只处理前一个命令正确输出，不处理错误输出
* 右边命令必须能够接收标准输入流，否则传递过程中数据会被抛弃
* sed awk grep cut head top less more wc join sort split等指令都是能接收标准输入流的指令

* 例子：

  * grep '内容\\[abc\\] info.log 这里的中括号需要进行转义 '
  * grep -o  '内容abc' 只匹配包含该内容的语句（匹配到用逗号隔开的语句），进一步过滤无关的内容
  * grep -v 'grep'  把包含该字符串的该行剔除，进一步过滤
  * 上面三句命令可以组合一个管道的操作符  一次性过滤掉无关的内容



