---
title: 常用代码 Java
tags: [Java]
favorited: true
pinned: true
---

# 常用代码 Java

## 非 null 赋值
```java
this.value = Objects.requireNonNull(value); // 如果 value 为 null 则抛出异常
```

## 使用 Optional 简化空指针判断
[Optional](https://www.toutiao.com/i6649195540640694788/) 类这是 Java 8 新增的一个类，用以解决程序中常见的 NullPointerException 异常问题。

下面为把用户名转换为大写的程序:

```java
User user = ...

if (user != null) {
    String userName = user.getUserName();
    if (userName != null) {
        return userName.toUpperCase();
    }
}

return null;
```

使用 Optional 以简化成:
```java
User user = ...;

Optional<User> userOpt = Optional.ofNullable(user);
String name = userOpt.map(User::getUsername).map(String::toUpperCase).orElse(null);
```
关注一下几点:
* `Optional.of(user)` 和 `Optional.ofNullable(user)` 的区别是 user 为 null 时 `of()` 抛空指针异常，`ofNullable()` 不会
* `orElse()`: 如果有值就返回，否则返回一个给定的值作为默认值
* `Optional.isPresent()` 和 `Optional.get()`: 
  * user 等于 null 时 `userOpt.isPresent()` 返回 false，调用 `userOpt.get()` 获取存储的 user 对象时抛出空指针异常
  * user 不为 null 时 `userOpt.isPresent()` 返回 true
* `Optional.map()` 就是 Lambda 的 map，对数据类型进行映射转换

## CyclicBarrier
[CyclicBarrier](https://www.toutiao.com/i6640482066855100931) 的内部利用 ReentrantLock 锁和关联的 Condition 条件队列来实现等待和唤醒的, CyclicBarrier 根据一个倒数计数来判断应该阻塞还是唤醒，类似于 CountDownLatch:
1. 创建一个 CyclicBarrier: `b = new CyclicBarrier(5)`
2. 当 5 个线程都调用了 `b.await()` 时一起往下执行
2. 如果前 4 个线程都执行到了 `b.await()`
3. 第 5 个线程却没有调用 `b.await()`，而是遇到问题，转而去调用了 `b.reset()`, 那么，前面 4 个阻塞在 `await()` 方法上的线程将抛出 BrokenBarrierException 异常

<img src="@attachment/cyclic-barrier.jpg" width=300>

## Excel 获取单元格的值
```java
float qs = getCellValue(sheet, 1, i, validateResult, Float.class);
```
```java
import org.apache.poi.ss.usermodel.Cell;
import org.apache.poi.ss.usermodel.Row;
import org.apache.poi.ss.usermodel.Sheet;
import org.apache.poi.ss.usermodel.Workbook;

/**
 * 从单元格中获取值并通过反射转换为指定的类型
 *
 * @param sheet  Excel 表格
 * @param rowNum 行号
 * @param colNum 列号
 * @param result 验证结果
 * @param c      返回类型
 * @return 单元格中的数据
 */
private <T> T getCellValue(Sheet sheet, int rowNum, int colNum, Set<String> result, Class<T> c) throws Exception {
    // 1. 定义指定参数类型的构造函数
    // 2. 判断指定行是否存在，不存在默认返回 0
    // 3. 判断指定单元格是否存在，不存在默认返回 0
    // 4. 判断单元格格式是否正确，不存在默认返回 0
    // 5. 如果是整数去掉 POI 在末尾自动追加的小数，返回对应类型结果

    // [1] 定义指定参数类型的构造函数
    Constructor<T> constructor = c.getConstructor(String.class);

    // [2] 判断指定行是否存在，不存在返回默认值 0
    Row row = sheet.getRow(rowNum);
    if (row == null) {
        result.add("第 " + (rowNum + 1) + " 行没有数据但有格式，请在 Excel 中删除再重新导入<br>");
        return constructor.newInstance("0");
    }

    // [3] 判断指定单元格是否存在，不存在默认返回 0
    Cell cell = row.getCell(colNum);
    if (cell == null) {
        result.add("第 " + (rowNum + 1) + " 行" + "第 " + (char) (colNum + 65) + " 列没值<br>");
        return constructor.newInstance("0");
    }

    // [4] 判断单元格格式是否正确，不正确默认返回 0
    String value = cell.toString();
    if ((c.equals(Double.class) || c.equals(Integer.class) || c.equals(Float.class)) && !value.matches("[\\d.]+")){
        result.add("第 " + (rowNum + 1) + " 行" + "第 " + (char) (colNum + 65) + " 列格式不正确<br>");
        return constructor.newInstance("0");
    }

    // [5] 如果是整数去掉 POI 在末尾自动追加的小数，返回对应类型结果
    if (c.equals(Integer.class)) {
        value = value.split("\\.")[0];
    }

    return constructor.newInstance(value);
}
```

## 优先级队列
[优先队列](https://www.toutiao.com/i6635540435437617671/)的作用是能保证每次取出的元素都是队列中权值最小的 (Java 的优先队列每次取最小元素，C++ 的优先队列每次取最大元素), 可以提供 Comparator 定义出队的权重比较, 默认使用小顶堆实现:
* 入队: `offer()`
* 出队: `poll()`
* 队首: `peek()`

```java
public static void main(String[] args) throws IOException {
    PriorityQueue<String> queue = new PriorityQueue<>((a, b) -> b.length() - a.length());
    queue.offer("One");
    queue.offer("Two");
    queue.offer("Three");
    queue.offer("Four");
    queue.offer("A");

    // 能保证每次取出的元素都是队列中权值最小的
    for (String e : queue) {
        System.out.println(e);
    }

    System.out.println("--------------");

    while (!queue.isEmpty()) {
        System.out.println(queue.poll());
    }
}
```

## Lambda
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
        //    toCollection(Supplier)
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

## 解析 HTML

使用 [Jsoup](https://jsoup.org/cookbook/input/parse-document-from-string) 解析 HTML:
```java
String html = "<html><head><title>First parse</title></head>"
  + "<body><p>Parsed HTML into a doc.</p></body></html>";
Document doc = Jsoup.parse(html);
doc.select("p").addClass("chapter");
```
```java
Document doc = Jsoup.connect("http://jsoup.org").get();

Element link = doc.select("a").first();
String relHref = link.attr("href"); // == "/"
String absHref = link.attr("abs:href"); // "http://jsoup.org/"
```
