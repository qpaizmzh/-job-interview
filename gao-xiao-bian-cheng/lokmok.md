# Lokmok

* 一个常用于将目前的代码样板生成的注解java库，有效地调高了开发的效率
* 常用的注解有@getter,@setter,@toString，@EqualsAndHashCode，@Data\(包含了前面几个的注解\)等常用的框架，一些需要特别地注意一下：

```java
   public void fileautoclose() throws IOException {
        //直接添加这里的注解，不需要书写关闭的连接
        @Cleanup InputStream inputStream = new FileInputStream("");
        @Cleanup OutputStream outputStream = new FileOutputStream("");
        byte[] b = new byte[100];
        int r = 0;
        while ((r = inputStream.read(b, 0, r)) != -1) {
            outputStream.write(b);
        }
```



