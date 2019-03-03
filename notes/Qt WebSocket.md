---
title: Qt WebSocket
tags: [Qt/Net]
created: '2019-01-07T08:56:11.314Z'
modified: '2019-03-02T01:30:18.982Z'
---

# Qt WebSocket

```cpp
int main(int argc, char *argv[]) {
    QApplication app(argc, argv);

    for (int i = 300000; i < 306000; ++i) {
        QWebSocket *socket = new QWebSocket;
        socket->open(QUrl(QString("ws://192.168.10.130:3721?userId=%1&username=Biao").arg(i)));
    }

    return app.exec();
}
```
