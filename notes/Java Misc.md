---
title: Java Misc
tags: [Java]
created: '2019-03-19T04:48:50.742Z'
modified: '2019-03-23T13:19:44.247Z'
---

# Java Misc

基于链接的授权：
  * Spring Security 中设置所有链接都需要角色 USER 才能访问
  * AuthenticationFilter 获取用户登录信息时 (基于 token) 查询用户是否可以访问此链接，如果可以设置角色为 USER 使得其有权访问，否则设置为没有权限的用户角色如 NO 即可

时序图:
  * 同步消息: 实线、实心箭头 ->
  * 异步消息: 实线、空心箭头 ->>
  * 返回消息: 虚线、空心箭头 -->>

---

偏应用 (partial application) 可以被描述为一个函数，它接受一定数目的参数，绑定值到一个或多个这些参数，并返回一个新的函数，这个返回函数只接受剩余未绑定值的参数。返回的函数称为偏应用函数。
```js
<script>
    var people = { name: 'alice', age: 23 };

    function func(greeting, fox) {
        console.log(greeting + ' - ' + this.name + ' - ' + fox); // this 是什么呢
    }

    var pf = _.bind(func, people, 'hi'); // 偏应用函数
    pf('bar');

    var add = function(a, b) { return a + b; };
    add5 = _.partial(add, 5);
    add5(10); // 15
</script>
```

---

Auth 登录框架 [Just Auth](https://gitee.com/jorneyr/JustAuth)，同类型的有 [Scribe Java](https://github.com/scribejava/scribejava)。
