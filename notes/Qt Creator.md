---
title: Qt Creator
tags: [Qt]
created: '2019-03-03T01:42:45.839Z'
modified: '2019-03-03T01:59:10.892Z'
---

# Qt Creator
新版的 Qt Creator 对代码进行了更严格的检查，老项目可能会有很多的警告

<img src="../attachments/qt/old-style-cast.png" width=1024>

如果不想看到这些警告，可以修改配置进行关闭: `配置 -> C++ -> Code Model -> Manage...`。例如想去掉 `-Wold-style-cast` 的警告，增加 `-Wno-old-style-cast` 即可。
> `no-`作为前缀
