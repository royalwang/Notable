---
title: Qt Pro 文件
tags: [Qt/Core]
---

# Qt Pro 文件

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
