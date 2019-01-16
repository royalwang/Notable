---
title: 常用代码 Java
tags: [Java]
favorited: true
pinned: true
---

# 常用代码 Java

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
