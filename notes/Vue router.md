---
title: Vue router
tags: [Vue]
created: '2019-02-06T05:13:56.063Z'
modified: '2019-02-06T13:55:06.052Z'
---

# Vue router
有 2 个变量, `this.$route` 和 `this.$router`，他们是不一样的:
* route: 跳转后，获取参数
* router: 发起跳转

## 路由配置
router.js 中配置路由:
* `:clazzId` 是路径 path 中的变量
* name 是 path 的名字，为了方便引用，不需要再写完整的 path

```js
{
    // 考试分析
    path: '/clazzes/:clazzId/analyais-learn-teaching',
    name: 'analyais-learn-teaching',
    component: () => import('./subpage/analysis/analyais-learn-teaching.vue'),
}
```

## 页面链接
在页面中使用 `<router-link>` 生成链接 `<a>`:
* name 的值为 router.js 中配置的 name
* params 设置 router.js 中配置的 path 中的参数，value 是 data 中的属性
* query 为 URL 中问号后面的参数

```html
<router-link :to="{ name: 'analyais-learn-exam', params: { clazzId: clazzId }, query: { days: days } }" slot="extra" class="text-icon">
    查看详情 <Icon type="ios-arrow-forward"/>
</router-link>
```

```js
data() {
    return {
        clazzId: 0, // 班级 ID
    };
},
```

## 程序跳转
```js
this.$router.push({ name: 'analyais-learn-teaching', params: { clazzId: this.clazzId } });
```

## 参数获取
获取路径中的参数:
* `clazzId` 是 router.js 中配置的 path 中的 `:clazzId`
* `days` 是 URL 问号后面的参数，没有在 path 中配置

```js
created() {
    this.clazzId = this.$route.params.clazzId;
    this.days    = this.$route.query.days;
}
```
