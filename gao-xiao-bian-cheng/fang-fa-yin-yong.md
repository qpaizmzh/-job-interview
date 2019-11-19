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

* 指向任意类型的实例方法的引用

```java
    /***
     * （args）->args.instanceMethod()
     * ClassName::instanceMethod
     */
    public void te() {
        Consumer<String> consumer = (String number)->number.length()
        Consumer<String> consumer2 = String::length;
    }
```

* 指向现有对象的实例方法的方法引用

```java
/***
* （args）->object.instanceMethod(args)
* object::instanceMethod
*/
 public void te() {
        StringBuilder b = new StringBuilder();
        Consumer<String> consumer = (String number) -> b.append(number);
        Consumer<String> consumer2 = b::append;
    }
```



