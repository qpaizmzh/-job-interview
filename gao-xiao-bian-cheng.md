# 函数编程

* 根据业务的变更和数量的不断的更替，实际上可以抽象出一个接口出来，专门负责处理该类型的业务：

```java
//将业务逻辑写死在方法里（处理方式的不断演变）
    public static List<String> filterString(List<String> stringList) {
        List<String> newList = new ArrayList<>();
        for (String bs :
                stringList) {
            //直接把逻辑写到方法的里面
            if (bs.equals("electronic")) {
                newList.add(bs);
            }
        }
        return newList;
    }

  //添加了枚举类的参数，根据传入的类型筛选指定的list
    public static List<Skt> filterStringwithenum(List<Skt> stringList, StringCatogoryEnum stringCatogoryEnum) {
        List<Skt> newList = new ArrayList<>();
        for (Skt bs :
                stringList) {
            //使用传入的类型来指定特定符合条件的对象
            if (stringCatogoryEnum.electronic.equals(bs.getCatogory())) {
                newList.add(bs);
            }
        }
        return newList;
    }
    
    //一开始可以写一些类实现这个接口，创建这一个的逻辑，但是书写的代码量也是很大，而且用的频率不高
    public static List<Skt> filterStringwithenum(List<Skt> stringList, CatgorySkt test) {
        List<Skt> newList = new ArrayList<>();
        for (Skt bs :
                stringList) {
            //使用传入的类型来指定特定符合条件的对象
            if (test.test(stringList)) {
                newList.add(bs);
            }
        }
        return newList;
    }
//不妨在调用这个方法的时候直接实现这个接口：
 filterStringwithInteface(stringList, new CatgorySkt() {
            @Override
            public boolean test(List<Skt> sktList) {
            //实现方式自己写
                return false;
            }
        });
//进一步简化，就有了对应的lambda表达式
 @Test
    public void givemeTest(){
        filterStringwithInteface(new ArrayList<Skt>(), (Skt skts)->skts.getCatogory().equals(""));
    }
```



