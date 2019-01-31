---
title: Vuex
tags: [Vue]
created: '2019-01-13T12:17:30.086Z'
modified: '2019-01-31T02:59:54.276Z'
---

# Vuex

![](../attachments/vuex.png)

## 常量函数名
函数名定义为常量, 在 commit 和 dispatch 的时候使用常量进行调用, 有错误时容易发现问题.
```js
const CALCULATE = 'calculate';

const obj = {
    [CALCULATE]() {
        console.log('Hello');
    }
};

obj[CALCULATE]();
```
