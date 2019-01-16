---
title: FastJson
tags: [Java]
---

# FastJson

## 反序列化 Boolean
JSON 字符串反序列化为 Java 对象时, 不只是 true 和 false 能够转换为 boolean 变量的值:
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
{ "visible": true }    // true
{ "visible": false }   // false
{ "visible": 1 }       // true
{ "visible": 0 }       // false
{ "visible": 3 }       // false
{ "visible": "true" }  // true
{ "visible": "True" }  // true
{ "visible": "false" } // false
{ "visible": "falsE" } // false
{ "visible": " true" } // Exception
{ "visible": "Bla" }   // Exception
```
