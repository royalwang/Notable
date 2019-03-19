---
title: 常用代码 Qt
tags: [Qt/Core]
pinned: true
created: '2019-01-12T13:10:31.936Z'
modified: '2019-03-17T14:57:45.121Z'
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

## 初始化列表
有了 std::initializer_list 之后，对于 STL 的 container 的初始化就方便多了, 比如以前初始化一个 vector 需要这样：

```cpp
std::vector v;
v.push_back(1);
v.push_back(2);
v.push_back(3);
v.push_back(4);
```

而现在 c++11 添加了 std::initializer_list 后，我们可以这样初始化:

```cpp
std::vector v = { 1, 2, 3, 4 };

QStringList list {"One", "Two", "Three"};
QStringList list = {"One", "Two", "Three"};
```

我们也可以使用迭代器访问 std::initializer_list 里的元素:

```cpp
for(auto i = l.begin(); i != l.end(); ++i) {
      cout << *i << " ";
}
```

## 鼠标穿透
在 Qt 中，QWidget 默认是非鼠标穿透的，如果将 QWidget 覆盖在其他控件上面，即使这个 QWidget 是透明的，鼠标也是无法点击下面的控件的。但是我们可以通过设置它的属性来实现鼠标穿透:

```cpp
QWidget::setAttribute(Qt::WA_TransparentForMouseEvents, true);
```


## 信号槽重载
```cpp
// QOverload<> 里面是参数列表，of() 里面是成员函数地址
QObject::connect(comboBox, QOverload<const QString &>::of(&QComboBox::activated), [](const QString &text) {
    qDebug() << text;
});
```

## Misc
* 启用 QSS: `this->setAttribute(Qt::WA_StyledBackground)`
* 显示提示: `QToolTip::showText(QCursor::pos(), "Hallo")`
