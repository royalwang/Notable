---
title: FastJson
tags: [Java]
created: '2019-01-12T07:29:40.685Z'
modified: '2019-03-10T13:58:49.702Z'
---

# FastJson

## 反序列化 Boolean
JSON 字符串反序列化为 Java 对象时, boolean、数字、字符串能够转换为 boolean 变量的值:
* boolean:
  * true: true
  * false: false
* 数字:
  * 1: true
  * 0: false
  * 其他数字: false
* 字符串:
  * "true": true (大小写不敏感)
  * "false": false (大小写不敏感)
  * "1": true
  * "0": false
  * 其他字符串抛异常

```json
// true
{ "visible": true }    // true
{ "visible": 1 }       // true
{ "visible": "true" }  // true
{ "visible": "True" }  // true

// false
{ "visible": false }   // false
{ "visible": 0 }       // false
{ "visible": 3 }       // false
{ "visible": "false" } // false
{ "visible": "falsE" } // false

// exception
{ "visible": " true" } // Exception
{ "visible": "Bla" }   // Exception
```
