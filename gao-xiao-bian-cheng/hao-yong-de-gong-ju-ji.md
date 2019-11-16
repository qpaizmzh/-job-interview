# 好用的工具集

## Optional的使用

```java
 public void optional() {
        //创建空的Optional的对象
        Optional.empty();
        //使用非null值创建Optional的对象，如果是null会抛异常
        Optional.of("张");
        //接受包括null值的
        Optional<Object> optional = Optional.ofNullable(null);

        //尽量使用ispresent方法判断是否为空，这样的判断和自己写非空判断没有区别
        //建议这样使用,直接使用这个方法，传递相应的方法体
        optional.ifPresent(System.out::println);

        /***
         * 当引用缺失（即检查对象为null）的时候这样使用
         */
        //引用缺失时返回括号里面的值
        optional.orElse("");
        //引用缺失时传入一个函数返回特定的值
        optional.orElseGet(
                () -> {
                    return "自定义返回的值"
                }
        );
        //引用缺失的时候抛出一个异常
        optional.orElseThrow(() -> {
            throw new RuntimeException("异常");
        });

    }

    public static void stream(List<String> list){
        list.stream().forEach(System.out::println);//如果list是null,会直接报错
        //可以使用Optional来规避相应的空指针异常
        Optional.ofNullable(list)
                .map(List::stream)
                .orElseGet(Stream::empty)
                .forEach(System.out::println);
    }
```



