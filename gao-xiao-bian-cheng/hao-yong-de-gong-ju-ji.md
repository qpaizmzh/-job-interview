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

# 不可变集合的使用

* ## 使用guava包代替JDK提供的Collections.unmodifiedXXX\(\)方法要好![](/高效编程/不可变集合2.png)MultiSet
* 一个可重复的无序集合

* 实例使用一个MultiSet集合类，实现统一篇文章中出现次数的功能

```java
   public void test() {
        Multiset<Character> multiset = HashMultiset.create();

        char[] chars = text.toCharArray();
        //Chars也是工具包的一部分
        Chars.asList(chars)
                .stream()
                .forEach(character -> multiset.add(character));
        //统计总共的字符的大小
        System.out.println("size:"+multiset.size());
        //统计你想匹配的字符的次数
        System.out.println("count:"+multiset.count('h'));
    }
```

## Set和List常用的方法

```java
 private static final Set set1 = Sets.newHashSet(1, 2, 3);

    private static final Set set2 = Sets.newHashSet(4, 5, 6);

    //并集方法
    @Test
    public void union() {
        Set<Integer> set = Sets.union(set1, set2);
        System.out.println(set);
        //结果是[1,2,3,4,5,6]
    }

    //交集
    @Test
    public void intersection() {
        Set<Integer> set = Sets.intersection(set1, set2);
        System.out.println(set);
        //结果是【】
    }

    //差集：如果元素属于A集而且不属于B集
    @Test
    public void difference() {
        Set<Integer> set = Sets.difference(set1, set2);
        System.out.println(set);
        //结果是[1, 2, 3]

        //相对差集，如果两个集合是[1,2,3,4]和[4,5,6]，则返回的结果就是[1,2,3,5,6]
        Set<Integer> sets = Sets.symmetricDifference(set1, set2);
        System.out.println(sets);
    }

    //拆分所有子集合
    @Test
    public void powerSet() {
        Set<Set<Integer>> set = Sets.powerSet(set1);
        System.out.println(JSON.toJSONString(set));
        //结果是[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

    }

    //计算笛卡尔积
    @Test
    public void cartesianProduct() {

        Set set = Sets.cartesianProduct(set1, set2);
        System.out.println(JSON.toJSONString(set));
        //结果是[[1,4],[1,5],[1,6],[2,4],[2,5],[2,6],[3,4],[3,5],[3,6]]
    }

    //List提供的方法
    //集合分段的方法
    @Test
    public void partition() {

        List<Integer> integers = Lists.newArrayList(1, 2, 3, 4, 5, 6);
        List<List<Integer>> partition = Lists.partition(integers, 3);
        System.out.println(JSON.toJSONString(partition));
        //结果是[[1,2,3],[4,5,6]]
    }

    //翻转
    @Test
    public void reverse() {

        List<Integer> integers = Lists.newArrayList(1, 2, 3, 4, 5, 6);
        List<Integer> list = Lists.reverse(integers);
        System.out.println(JSON.toJSONString(list));
        //结果是[6,5,4,3,2,1]
    }
```



