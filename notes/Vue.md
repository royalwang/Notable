---
title: Vue
tags: [Vue]
created: '2019-01-13T12:24:16.914Z'
modified: '2019-02-06T07:58:42.964Z'
---

# Vue

## class
三元表达式绑定 class，要用数组:

```html
<div :class="[examAvgPassRateUp ? 'up' : 'down']">
```

使用对象绑定 class，其中 value 是 boolean 属性:

```html
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
```

<img src="../attachments/vue-lifecycle.png" width=600>
