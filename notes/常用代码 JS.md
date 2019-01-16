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
`Object.assign()` 后面的参数属性覆盖前面参数的同名属性:

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
