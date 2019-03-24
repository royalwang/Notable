---
title: Java 8 时间
tags: [Java]
created: '2019-01-31T06:52:42.217Z'
modified: '2019-03-19T07:05:56.134Z'
---

# Java 8 时间
```java
// 世界标准时间 UTC (coordinated universal time)，其中 T 表示时分秒的开始 (或者日期与时间的间隔)，Z (zero) 表示这是一个世界标准时间 (0 时区)
2017-12-13T01:47:07.081Z

// 本地时间，也叫不含时区信息的时间，末尾没有 Z
2017-12-13T09:47:07.153

// 含有时区信息的时间，+08:00 表示该时间是由世界标准时间加了 8 个小时得到的，[Asia/Shanghai] 表示时区
2017-12-13T09:47:07.153+08:00[Asia/Shanghai]
```

Java 8 中引入的时间类主要有下面几个:
* Instant: 时间戳
  * Instant.now()
  * 就是 java.util.Date: Date.from(Instant), Date.toInstant()
  * Instant.now().toEpochMilli()，等价于 System.currentTimeMillis()
* LocalDate: 只有年月日
* LocalTime: 只有时分秒
* LocalDateTime: 有年月日时分秒
* ZonedDateTime: 有年月日时分秒，并包含了时区信息

> 它们都有函数 `now()` 创建系统当前时间对象。

本地时间 `Local*` (电脑任务栏上显示的时间) 没有包含时区信息，具体表示哪里的时间全靠输入和输出时进行解释。
Instant 和 ZonedDateTime 等价，Instant 本身实际上就指明时区了，0 时区 (UTC: 世界标准时间)，ZonedDateTime 是含有时区信息的时间，本质上是根据时区对 Instant 的格式化显示。

```java
System.out.println(LocalDateTime.now()); // 当前时区: 2019-01-31T15:40:49.825
System.out.println(ZonedDateTime.now()); // 当前时区: 2019-01-31T15:40:49.826+08:00[Asia/Shanghai]
System.out.println(Instant.now());       // 0 时区: 2019-01-31T07:40:49.733Z
System.out.println(ZonedDateTime.now(ZoneId.of("+00:00"))); // 0 时区: 2019-01-31T07:40:49.826Z

System.out.println(LocalDateTime.of(970,  1, 1, 0, 0, 0).toEpochSecond(ZoneOffset.UTC)); // -31556908800
System.out.println(LocalDateTime.of(1970, 1, 1, 0, 0, 0).toEpochSecond(ZoneOffset.UTC)); // 0
System.out.println(LocalDateTime.of(2970, 1, 1, 0, 0, 0).toEpochSecond(ZoneOffset.UTC)); // 31556995200
```

字符串 `2019-01-31T15:10:35.818` 现在传给 0 时区和 8 时区的 2 台电脑使用 LocalDateTime 解析，得到的时间戳差 8 个小时 (使用系统当前的时区进行解析)。

字符串 `2019-01-31T15:10:35.819+08:00[Asia/Shanghai]` 现在传给 0 时区和 8 时区的 2 台电脑使用 ZonedDateTime 解析，得到的时间戳是一样的。

## 格式化和解析时间

格式化: 
```java
DateTimeFormatter.ofPattern("yyyy-MM-dd").format(LocalDate.now());
```
> 其实不需要格式化，因为输出时直接是 `2019-01-31T14:55:39.730` 这样的格式。

解析时间:
```java
LocalDateTime.parse("2017-12-13 10:10:10", DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));
```

参考 [Java 中的时间与时区](https://blog.csdn.net/u012107143/article/details/78790378)
