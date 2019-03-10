---
title: Qt Pro 文件
tags: [Qt/Core]
created: '2019-01-12T13:11:26.329Z'
modified: '2019-03-07T07:18:05.713Z'
---

# Qt Pro 文件

## 普通工程

```
TARGET   = WebSocketTester
TEMPLATE = app

CONFIG  -= app_bundle
RC_ICONS = AppIcon.ico  # For Win
ICON     = AppIcon.icns # For Mac

DEFINES += QT_MESSAGELOGCONTEXT
DEFINES += QT_DEPRECATED_WARNINGS

# Output directory
CONFIG(debug, debug|release) {
    output = debug
    TARGET = $$TARGET'_d'
}
CONFIG(release, debug|release) {
    output = release
}

DESTDIR     = bin
OBJECTS_DIR = $$output
MOC_DIR     = $$output
RCC_DIR     = $$output
UI_DIR      = $$output
```

## 子项目工程

`子工程模式`中，把下面公共的 `common.pri` 放到项目根目录下，子项目的 pro 中 `include(common.pri)`:
```ini
CONFIG  += c++11

DEFINES += QT_MESSAGELOGCONTEXT
DEFINES += QT_DEPRECATED_WARNINGS

# 工程根目录，子项目里头文件可以使用 #include <MyLibrary/xxx.h> 的方式包含
project      = $$PWD
INCLUDEPATH += $$project

# 编译输出目录
CONFIG(debug, debug|release) {
    bin    = $$OUT_PWD/../bin-debug
    output = $$OUT_PWD/debug
}
CONFIG(release, debug|release) {
    bin    = $$OUT_PWD/../bin-release
    output = $$OUT_PWD/release
}

DESTDIR     = $$bin
MOC_DIR     = $$output
RCC_DIR     = $$output
UI_DIR      = $$output
OBJECTS_DIR = $$output
```

> 每个子项目编译出的中间文件都输出到各自的目录下，编译出的动态链接库和可执行文件输出到同一个目录下。

`动态链接库子项目`的 pro:
```ini
include(../common.pri)

QT      -= gui
TARGET   = MyLibrary
TEMPLATE = lib

SOURCES += \
    Calculator.cpp

HEADERS += \
    Calculator.h
```

`可执行子项目`的 pro:
```ini
include(../common.pri)

QT      = gui widgets
CONFIG -= app_bundle

# 库文件引入: 在输出目录
LIBS += -L$$bin -lMyLibrary

SOURCES += main.cpp
```

> 注意: `$$PWD` 是 pri 文件所在目录 (不随子项目变化)，`$$OUT_PWD` 是项目编译输出目录 (随子项目变化)，在子项目工程中注意它们输出的值。
