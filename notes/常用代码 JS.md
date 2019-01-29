---
title: 常用代码 JS
tags: [JS]
pinned: true
created: '2019-01-12T13:10:25.537Z'
modified: '2019-01-25T22:44:40.792Z'
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

const arr = [1, 2, 3, 4, 1, 2];
const set = new Set(arr); // 传入数组创建 Set
const str = Array.from(set).join(', ');
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

## 解构赋值
从对象中提取所请求的属性，并将其分配给与属性相同名称的变量:

```js
// 可以有默认参数
function fox({ id = 123, username }) {
    console.log(id, username);
}

fox({ username: 'Alice' });
```

```js
const { data } = await axios.get(...);
const { data: newData } = await axios.get(...); // data 重命名
const { id = 5 } = {}; // 解构时还可以赋值，避免 undefined
```

## call 和 reply
`func.call(obj, args)` 等价于 `obj.func(args)` (obj 可能没有函数 func)，func() 中的 this 等于 obj。
