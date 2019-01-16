---
title: 常用代码 Qt
tags: [Qt/Core]
favorited: true
pinned: true
---

# 常用代码 Qt

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
