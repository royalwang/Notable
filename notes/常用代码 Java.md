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
关注以下几点:
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
