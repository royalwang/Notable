---
title: 常用代码 JS
tags: [JS]
favorited: true
pinned: true
---

# 常用代码 JS

## 毫秒数
```js
var milli = Date.now();
var milli = new Date().getTime();
```

## 随机数
```js
const rand = Math.floor(Math.random() * 1000); // [0, 100)
```

## For 循环
可用 `forEach` 或者 `for of`:

```js
var ns = [1, 2, 3, 4];

ns.forEach(n => {
    console.log(n);
});

for (let n of ns) {
    console.log(n);
}
```

## 合并对象
`Object.assign()` 后面的参数属性覆盖前面参数的同名属性, 只有一层, 浅拷贝:

```js
var defaults = {
    name: 'default-name',
    mail: 'default@gmail.com',
}

var config = {
    id: 1,
    name: 'Alice',
}

var result = Object.assign({}, defaults, config);
console.log(JSON.stringify(result)); // 输出: { "id": 1, "name": "Alice","mail": "default@gmail.com" }
```

## 数组和集合
```js
const phaseSet = new Set();
phaseSet.add(clazz.phase);
Array.from(phaseSet);
```

## 字符串
[字符串的函数列表](https://www.w3schools.com/jsref/jsref_obj_string.asp):
* endsWith()
* includes()
* indexOf()
* match()
* replace()
* split()
* startsWith()
* toLowerCase()
* toUpperCase()
* trim()

包含子字符串:
```js
var str = 'Hello world';
if (str.includes('llo')) {

}
```

大小写转换:
```js
str.toUpperCase();
str.toLowerCase();
```
