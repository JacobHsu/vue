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


## vue的[router-view如何传递props](https://segmentfault.com/q/1010000005362842)  


多級路由，所以問題就在這了，從最外層的<router-view>設置props，
想在下面的某一級sub-router接收，那麻煩點，你得在中間路過的每一級都把props透傳下去，直至你最裡層的那個組件。

一個三級的[jsfiddle範例](http://jsfiddle.net/leftstick/L50pdx95/)，最外層放:room，中間透傳，三層組件Bar接收。  

App 一開始設定 data `room`  

`<router-view :room="room"></router-view>`  

app.vue
```js
<a v-link="{ path: '/foo/bar' }">Go to /foo/bar</a>
```

router.js
```js
subRoutes: {
    '/bar': {
         component: Bar
     },    
```

app.vue  
```js
var Bar = Vue.extend({
    props: {
        room: {
            type: Array,
            default: function(){
                return [];
            }
        }
    },
    template: '<p v-for="v of room" track-by="$index">This is bar!{{ v }} </p>'
})
```