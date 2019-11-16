# 方法引用

* 本质是个语法糖
* 指向静态方法的引用

```java
     /***
     * （args）->ClassName.staticMethod(args)
     * ClassName::staticMethod
     */
  public void te() {
        Consumer<String> consumer = (String number)->Integer.parseInt(number);
        Consumer<String> consumer2 = Integer::parseInt;
    }
```

* 


