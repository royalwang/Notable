---
title: 常用代码 Qt
tags: [Qt/Core]
pinned: true
created: '2019-01-12T13:10:31.936Z'
modified: '2019-02-28T13:51:55.846Z'
---

# 常用代码 Qt

[C++ 11](http://blog.jobbole.com/44015/)

## UUID
```cpp
QString uuid = QUuid::createUuid().toString().replace("{", "").replace("}", "").replace("-", "").toUpper();
```

## MD5
```cpp
QString md5 = QCryptographicHash::hash("Biao", QCryptographicHash::Md5).toHex();
```

## 计时器
```cpp
QTimer *timer = new QTimer();

connect(timer, &QTimer::timeout, [] {
    qDebug() << "Hello";
});

timer->start(10000); // 每 10s 发射一次 timeout 信号
```

## 线程安全的整数
```cpp
QAtomicInt count = 0;
count++;
count--;
```

## 随机数
Qt 中推荐使用 QRandomGenerator 生成随机数, 而不再推荐使用 qrand():
```cpp
qint32 value = QRandomGenerator::global()->generate();   // [0, MAX_INT)
qint32 value = QRandomGenerator::global()->bounded(100); // [0, 100)
```

## 进程间通讯
可以使用 `QLocalServer` + `QLocalSocket` 进行进程间通讯, Windows 下其实就是命名管道.
> On Windows this is a named pipe and on Unix this is a local domain socket.

可以用来实现 Single Application: 
* 启动程序
* 使用 QLocalSocket 尝试连接 QLocalServer
* 如果能连上, 说明已经启动了一个程序, 发送消息使得已经启动的程序成为当前桌面程序, 退出当前程序
* 如果连不上, 说明还没有启动, 启动程序并启动 QLocalServer 进行监听

共享内存 `QSharedMemory` 也可以进程间通讯, 但是不能主动的推送信息给对方, QLocalSocket 可以.

## 按键组合
`QKeySequence("Alt+P")`: Alt 和 P 之间不能有空格

## QMap
```cpp
// 遍历 Map
void func(QMap<QString, int> map) {
    // 方式一: 最简洁
    for (auto i = map.cbegin(); i != map.cend(); ++i) {
        qDebug() << i.key() << ": " << i.value();
    }

    // 方式二: 很 Java
    QMapIterator<QString, int> iter(map);
    while (iter.hasNext()) {
        iter.next();
        qDebug() << iter.key() << ": " << iter.value();
    }
}

int main(int argc, char *argv[]) {
    // 使用初始化列表创建 map
    QMap<QString, int> map {{"name", 1}, {"box", 2}};

    func(map);
    func({{"name", 1}, {"box", 2}});
}
```

## 鼠标穿透
在 Qt 中，QWidget 默认是非鼠标穿透的，如果将 QWidget 覆盖在其他控件上面，即使这个 QWidget 是透明的，鼠标也是无法点击下面的控件的。但是我们可以通过设置它的属性来实现鼠标穿透:

```cpp
QWidget::setAttribute(Qt::WA_TransparentForMouseEvents, true);
```

## Strongly-typed enums 强类型枚举

传统的 C++ 枚举类型存在一些缺陷：它们会将枚举常量暴露在外层作用域中（这可能导致名字冲突，如果同一个作用域中存在两个不同的枚举类型，但是具有相同的枚举常量就会冲突），而且它们会被隐式转换为整形，无法拥有特定的用户定义类型。

在 C++11 中通过引入了一个称为强类型枚举的新类型，修正了这种情况。强类型枚举由关键字 `enum class` 标识。它不会将枚举常量暴露到外层作用域中，也不会隐式转换为整形，并且拥有用户指定的特定类型 (传统枚举也增加了这个性质)
```cpp
enum class Option {
    One, Two, Three
}

Option o = Option::One;
```
