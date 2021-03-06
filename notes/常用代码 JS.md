---
title: 常用代码 JS
tags: [JS]
pinned: true
created: '2019-01-12T13:10:25.537Z'
modified: '2019-03-24T01:13:52.239Z'
---

# 常用代码 JS

## 毫秒数
```js
var milli = Date.now();
var milli = new Date().getTime();
```

## 随机数
```js
const rand = Math.floor(Math.random() * 1000); // [0, 1000)
```

## For 循环
可用 `for of` 或者 `forEach`:

```js
var ns = [1, 2, 3, 4];

for (let n of ns) {
    console.log(n);
}

ns.forEach(n => {
    console.log(n);
});
```

可用 `for in` 把一个对象的所有属性依次循环出来:
```js
var o = { name: 'Jack', age: 20, city: 'Beijing' };
for (var key in o) {
    console.log(key); // 'name', 'age', 'city'
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

## 数组
### 数组初始化
```js
const array = new Array(10).fill(0); // 10 个元素的数组，每个元素初始化为 0
```

### 数组和集合
[Set and Map](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/0014345007434430758e3ac6e1b44b1865178e7aff9082e000)
```js
const phaseSet = new Set();
phaseSet.add(clazz.phase);
Array.from(phaseSet);

const arr = [1, 2, 3, 4, 1, 2];
const set = new Set(arr); // 传入数组创建 Set
const str = Array.from(set).join(', ');
```

### 数组中的最大值
```js
const ns1 = [];
console.log(Math.max(...ns1, 100));

const ns2 = [1, 2];
console.log(Math.max(...ns2));
```

### 数组元素求和
```js
let ns = [1, 2, 3, 4, 5];
let sum = ns.reduce((acc, e) => acc + e); // 初始值为 0

--------------------------------------------

let array = [
    { name: 'a', total: 1 },
    { name: 'b', total: 2 },
    { name: 'c', total: 3 },
    { name: 'd', total: 4 },
];

// 如果是对象的数组，初始值必须要有 
// 如果没有初始值则取第一个元素作为初始值，但是因其不为数字，所以出错
let sum = array.reduce((acc, e) => acc + e.total, 0);
```

### Range
```js
const hours = [...Array(24).keys()]; // [0, 1, 2, ..., 23]
```

## 映射 Map
```js
// Map 的 key 可以是字符串，数字，对象
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
m.get('Michael'); // 95
m.set('Bob', 59);
```

> 添加元素:
> 数组: `push()`
> 集合: `add()`
> 映射: `set()`

## 对象解构赋值
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

## 固定小数位
```js
d = d.toFixed(2);
```

## Day.js
[Day.js](https://github.com/iamkun/dayjs) 是一个仅 2kb 大小的轻量级 JavaScript 时间日期处理库，[API 文档](https://github.com/iamkun/dayjs/blob/dev/docs/en/API-reference.md):
```js
<script src="https://unpkg.com/dayjs"></script>
<script>
    console.log(dayjs().format('YYYY-MM-DD HH:mm:ss'));
</script>
```

## Echarts
[Echarts 个性化图表的样式](https://www.echartsjs.com/tutorial.html#个性化图表的样式)
使用主题: `var chart = echarts.init(dom, 'light') or echarts.init(dom, 'dark')`
ECharts 中的数据项都是既可以只设成数值，也可以设成一个包含有名称:
```js
myChart.setOption({
    series : [
        {
            name: '访问来源',
            type: 'pie',
            radius: '55%',
            data:[
                {value:235, name:'视频广告'},
                {value:274, name:'联盟广告'},
                {value:310, name:'邮件营销'},
                {value:335, name:'直接访问'},
                {value:400, name:'搜索引擎'}
            ]
        }
    ]
})
```

## 字符串
[字符串的函数列表](https://www.w3schools.com/jsref/jsref_obj_string.asp):
* startsWith()
* endsWith()
* includes()
* indexOf()
* match()
* replace()
* split()
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

## Promise
```js
let p = new Promise((resolve, reject) => {
    setTimeout(() => {
        const time = parseInt(Math.random() * 100);

        // 异步代码
        if (time % 2 == 0) {
            resolve('even');
        } else {
            reject('odd');
        }
    }, 300);
});

let n = p.then((even) => {
    console.log(even);
}).catch((odd) => {
    console.log(odd);
});
```

## 延迟触发
函数在短时间内被多次频繁调用，只有在指定时间内不在被调用时才真正执行，可以使用 [underscore.js](https://www.html.cn/doc/underscore/) 来实现。
例如输入文字进行搜索，每输入一个字符就执行一次搜索的话短时间内就会进行大量的无用搜索，更好的方案是等待输入结束，如 300 毫秒内不再输入时才执行搜索:
```js
editor.on('change', _.debounce(() => {
    search(editor.getContent());
}, 300));
```
想象每天上班大厦底下的电梯。把电梯完成一次运送，类比为一次函数的执行和响应。假设电梯有两种运行策略 throttle 和 debounce ，超时设定为15秒，不考虑容量限制:
* throttle 策略的电梯: 保证如果电梯第一个人进来后，15 秒后准时运送一次，不等待。如果没有人，则待机
* debounce 策略的电梯: 如果电梯里有人进来，等待 15 秒。如果又人进来，15秒等待重新计时，直到15秒超时，开始运送

更信息的内容请参考 [浅谈 Underscore.js 中 _.throttle 和 _.debounce 的差异](https://segmentfault.com/a/1190000000755634)
> `console.log(_.functions(_))` 列出 underscore 的所有函数

## _.after
`_.after(count, function)`: 创建一个函数, 只有在运行了 count 次之后才有效果. 在处理同组异步请求返回结果时, 如果你要确保同组里所有异步请求完成之后才 执行这个函数, 这将非常有用。
