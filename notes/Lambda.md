---
title: Lambda
tags: [Java]
created: '2019-01-22T14:09:56.678Z'
modified: '2019-03-15T07:55:13.277Z'
---

# Lambda
使用 Lambda 的函数引用时, 只有一个参数, 并且只有一个语句:
* 调用参数自己的无参函数: `User::getName`
* 参数作为其他函数的参数: `System.out::println`

```java
import com.alibaba.fastjson.JSON;
import lombok.Getter;
import lombok.Setter;
import lombok.experimental.Accessors;

import java.util.*;
import java.util.stream.Collectors;

@Getter
@Setter
@Accessors(chain = true)
public class Lambda {
    private int id;
    private String name;

    public Lambda(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public static List<Lambda> prepareData() {
        List<Lambda> lms = new LinkedList<>();
        lms.add(new Lambda(1, "Alice"));
        lms.add(new Lambda(1, "Bob"));
        lms.add(new Lambda(1, "John"));
        lms.add(new Lambda(2, "Bob"));
        lms.add(new Lambda(2, "Steven"));
        lms.add(new Lambda(3, "John"));
        lms.add(new Lambda(3, "Loa"));

        return lms;
    }

    /**
     * 介绍 Lambda 的几个常用方法，更多的请参考 JDK 文档 Collectors
     * 只有 sort 会改变原来集合的数据，其他操作不会
     */
    public static void main(String[] args) {
        List<Lambda> lms = prepareData();

        // 1. 过滤 id 小于 3 的元素
        lms.stream().filter(e -> e.getId() > 2).forEach(e -> {
            System.out.println(JSON.toJSONString(e));
        });

        // 2. 获取所有不同的名字，返回 Set
        //    返回 List: Collectors.toList()
        //    Collectors.toCollection(Supplier)
        Set<String> names = lms.stream().map(Lambda::getName).collect(Collectors.toCollection(TreeSet::new));
        System.out.println(names);

        // 3. 合并为字符串
        System.out.println(names.stream().collect(Collectors.joining(", ")));
        System.out.println(String.join(", ", names));

        // 4. 排序: 先按名字升序排序，名字相同按 id 降序排序
        lms.sort(Comparator.comparing(Lambda::getName).thenComparing(Lambda::getId).reversed());
        System.out.println(JSON.toJSONString(lms));

        // 5. 使用 Map 分组，id 相同的放在一组
        // 6. 遍历 Map
        Map<Integer, List<Lambda>> groupedLms = lms.stream().collect(Collectors.groupingBy(Lambda::getId));
        groupedLms.forEach((id, lms2) -> {
            System.out.println(id + ": " + JSON.toJSONString(lms2));
        });

        // 7. List to Map
        Map<Integer, Lambda> lambdaMap = lms.stream().collect(Collectors.toMap(Lambda::getId, l -> l));
        // 注意: 如果 list 中有 2 个元素的 id 相同，则会报 duplicate key 的错误，解决这个问题可以给 toMap 第 3 个参数指定重复的时候使用哪一个元素
        lms.stream().collect(Collectors.toMap(Lambda::getId, l -> l, (oldValue, newValue) -> newValue));
    }
}
```

```java
int[] nums = new int[]{1, 2, 3, 4, 5, 6, 7, 8};
int min = IntStream.of(nums).min().getAsInt();
```

统计单词个数 (等价于 Scala 里的 `mapValues`):
```java
public static void main(String[] args) {
    List<String> list = Arrays.asList("Hello", "Hello", "World");
    Map<String, Long> map1 = list.stream().collect(Collectors.groupingBy(e->e, Collectors.counting()));
    System.out.println(map1); // {Hello=2, World=1}

    List<User> users = Arrays.asList(new User(1, "Biao"), new User(2, "Biao"), new User(3, "Alice"));
    Map<String, Long> map2 = users.stream().collect(Collectors.groupingBy(User::getUsername, Collectors.counting()));
    System.out.println(map2); // {Alice=1, Biao=2}
}
```
