---
title: Vue
tags: [Vue]
created: '2019-01-13T12:24:16.914Z'
modified: '2019-02-16T10:27:44.069Z'
---

# Vue

## class
三元表达式绑定 class，要用数组:

```html
<div :class="[examAvgPassRateUp ? 'up' : 'down']"></div>
<div :class="[examAvgPassRateUp ? 'up' : 'down', 'active']"></div>
```

使用对象绑定 class，其中 value 是 boolean 属性:

```html
<div class="static" v-bind:class="{ active: isActive, 'text-danger': hasError }"></div>
```
> 这个对象也可以是调用函数的返回值。

## filter
把 avgPassRate 转换为百分比的格式进行输出，例如 0.123 输出 12.3%

```html
{{ examData.avgPassRate | toPercent }}
```

```js
filters: {
    // 使用过滤器格式化输出
    // 把数字转换为百分比，signed 为 true，alue 大于等于 0 时，增加前缀 +
    toPercent(value, signed) {
        return Utils.numberToPercent(value, signed);
    },
    // 数字大于等于 0 时，增加前缀 +
    withSign(value) {
        return Utils.numberWithSign(value);
    }
},
```

## watch
```js
var vm = new Vue({
    el: '#demo',
    data: {
        firstName: 'Foo',
        lastName: 'Bar',
        fullName: 'Foo Bar'
    },
    watch: {
        firstName(val) {
            this.fullName = val + ' ' + this.lastName;
        },
        lastName(val) {
            this.fullName = this.firstName + ' ' + val;
        }
    }
});
```

输入停止 500ms 后才进行搜索，可以使用 lodash，例子参考 <https://cn.vuejs.org/v2/guide/computed.html#侦听器>。

## if else 
```html
<div v-if="type === 'A'">
    A
</div>
<div v-else-if="type === 'B'">
    B
</div>
<div v-else-if="type === 'C'">
    C
</div>
<div v-else>
    Not A/B/C
</div>
```

## directive
需要对普通 DOM 元素进行底层操作，这时候就会用到[自定义指令](https://cn.vuejs.org/v2/guide/custom-directive.html)。

```js
// 注册一个全局自定义指令 `v-focus`
Vue.directive('focus', {
    // 当被绑定的元素插入到 DOM 中时…… (inserted 是钩子函数，还有 update, bind 等)
    inserted: function (el) {
      // 聚焦元素
      el.focus()
    }
})
```

```js
// 组件内注册
directives: {
    // <input v-focus>
    focus: {
        // 指令的定义
        inserted: function (el) {
            el.focus()
        }
    },
    // <input v-todo-focus="todo==editedTodo"></input>
    'todo-focus': function(el) {
        el.focus()
    }
}
```

## 知识点
* 对于已经创建的实例，Vue 不能动态添加根级别的响应式属性。但是，可以使用 `Vue.set(object, key, value)` 方法向嵌套对象添加响应式属性。值得注意的是只有当实例被创建时 data 中存在的属性才是响应式的。如果你知道你会在晚些时候需要一个属性，但是一开始它为空或不存在，那么你仅需要设置一些初始值，也可以需要的时候使用 `this.$set(object, "fieldName", newValue)` 进行设置
* 注入到 Vue prototype 中的属性名推荐以 `$` 开头，用 `this.$name` 进行访问
* 不要在选项属性 (如 methods, [computed](https://cn.vuejs.org/v2/guide/computed.html)) 或生命周期回调函数上使用箭头函数，因为箭头函数是和父级上下文绑定在一起的，this 不会是如你所预期的 Vue 实例
* 双大括号会将数据解释为普通文本，而非 HTML 代码，HTML 代码需要使用 `v-html` 命令
* 对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript `表达式`支持: `{{ number + 1 }}, {{ ok ? 'YES' : 'NO' }}`，有个限制就是，每个绑定都只能包含单个表达式
* 对于任何复杂逻辑，你都应当使用计算属性
* [用 key 管理可复用的元素](https://cn.vuejs.org/v2/guide/conditional.html#用-key-管理可复用的元素): 这两个元素是完全独立的，不要复用它们
* 一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要`非常频繁地切换，则使用 v-show 较好`；如果在运行时条件很少改变，则使用 v-if 较好
* 当 v-if 与 v-for 一起使用时，v-for 具有比 v-if 更高的优先级
* v-for 指令需要使用 `item in items` 形式的特殊语法: `<li v-for="item in items">{{ item.message }}</li>`
* 有时也需要在内联语句处理器中访问原始的 DOM 事件。可以用特殊变量 `$event` 把它传入方法：`<button @click="warn('Bom', $event)">Button</button>`
* 组件是可复用的 Vue 实例，所以它们与 new Vue 接收相同的选项，例如 data、computed、watch、methods 以及生命周期钩子等。仅有的例外是像 el 是根实例特有的选项
* 组件定义中，data 必须是函数，因此每个实例可以维护一份被返回对象的独立的拷贝
* HTML 中的特性名是大小写不敏感的，所以浏览器会把所有大写字符解释为小写字符。这意味着当你使用 DOM 中的模板时，camelCase (驼峰命名法) 的 prop 名需要使用其等价的 kebab-case (短横线分隔命名) 命名，如果你使用字符串模板，那么这个限制就不存在了
  * 用在组件选项里用 template: "" 指定的模板
  * 单文件组件里用 `<template><template/>` 指定的模板
* 不同于组件和 prop，事件名不存在任何自动化的大小写转换。而是触发的事件名需要完全匹配监听这个事件所用的名称: 推荐你始终使用 kebab-case 的事件名
* 如果你想要将一个对象的所有属性都作为 prop 传入，你可以使用不带参数的 v-bind (取代 v-bind:prop-name): `<blog-post v-bind="post"></blog-post>`
* [单向数据流](https://cn.vuejs.org/v2/guide/components-props.html#单向数据流): 所有的 prop 都使得其父子 prop 之间形成了一个单向下行绑定：父级 prop 的更新会向下流动到子组件中，但是反过来则不行
* `.sync` 修饰符实现[双向绑定](https://cn.vuejs.org/v2/guide/components-custom-events.html#sync-修饰符)
* 插槽内可以包含任何模板代码，包括 HTML，甚至其它的组件。有的时候为插槽提供默认的内容是很有用的，如果父组件为这个插槽提供了内容，则默认的内容会被替换掉
* [在动态组件上使用 keep-alive](https://cn.vuejs.org/v2/guide/components-dynamic-async.html): `<keep-alive><component v-bind:is="currentTabComponent"></component></keep-alive>`，`is` 绑定的是组件的名字
* 使用 ref 访问子组件或者 DOM element
* [依赖注入 provide | inject](https://cn.vuejs.org/v2/guide/components-edge-cases.html#依赖注入): `provide` 选项允许我们指定我们想要提供给后代组件的数据/方法, 在任何后代组件里，我们都可以使用 `inject` 选项来接收指定的我们想要添加在这个实例上的属性，但所提供的属性是非响应式的 (需要响应式的需要使用 Vuex)
* [递归组件](https://cn.vuejs.org/v2/guide/components-edge-cases.html#递归组件): 组件是可以在它们自己的模板中调用自身的，可以用来创建树，但是记得使用 `v-if` 根据条件结束递归

计算属性是基于它们的依赖进行缓存的。只在相关依赖发生改变时它们才会重新求值。这也同样意味着下面的计算属性将不再更新，因为 Date.now() 不是响应式依赖：
```js
computed: {
    now() {
      return Date.now();
    }
}
```

## slot
未命名插槽 (是默认插槽)，它会作为所有未匹配到插槽的内容的统一出口

```html
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```
```html
<base-layout>
  <h1 slot="header">Here might be a page title</h1>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <p slot="footer">Here's some contact info</p>
</base-layout>
```

## [scoped-slot](https://cn.vuejs.org/v2/guide/components-slots.html#作用域插槽)

定义组件:
```html
<ul>
  <li
    v-for="todo in todos"
    v-bind:key="todo.id"
  >
    <!-- 我们为每个 todo 准备了一个插槽，-->
    <!-- 将 `todo` 对象作为一个插槽的 prop 传入。-->
    <slot v-bind:todo="todo">
      <!-- 回退的内容 -->
      {{ todo.text }}
    </slot>
  </li>
</ul>
```

使用组件:
```html
<todo-list v-bind:todos="todos">
  <template slot-scope="{ todo }">
    <span v-if="todo.isComplete">✓</span>
    {{ todo.text }}
  </template>
</todo-list>
```

## 生命周期
<img src="../attachments/vue-lifecycle.png" width=600>
