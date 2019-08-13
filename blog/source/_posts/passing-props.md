---
title: 路由組件傳參
---

[路由組件傳參](https://router.vuejs.org/zh/guide/essentials/passing-props.html#布尔模式)

如果 `props` 被設置為 `true`，`route.params` 將會被設置為組件屬性

router/index.js
```js
name: 'User'
component: User,
redirect: { name: 'UserMain' },
children: [
{
    path: 'user/:userID?/:userType?',
    name: 'UserMain',
    component: UserMain,
    props: true // 可用 `this.$route.params.userID` 
},
```

