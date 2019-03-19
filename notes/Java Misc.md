---
title: Java Misc
tags: [Java]
created: '2019-03-19T04:48:50.742Z'
modified: '2019-03-19T04:52:23.058Z'
---

# Java Misc

基于链接的授权：
  * Spring Security 中设置所有链接都需要角色 USER 才能访问
  * AuthenticationFilter 获取用户登录信息时 (基于 token) 查询用户是否可以访问此链接，如果可以设置角色为 USER 使得其有权访问，否则设置为没有权限的用户角色如 NO 即可

时序图:
  * 同步消息: 实线、实心箭头 ->
  * 异步消息: 实线、空心箭头 ->>
  * 返回消息: 虚线、空心箭头 -->>
