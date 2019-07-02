---
title: Vue.set
---

[Vue2 如何使用Vue.set?](https://segmentfault.com/q/1010000008472683)

```js
const app = new Vue({
   //data中已經存在info根屬性
  data: {
    a: 1
  }
  // render: h => h(Suduko)
}).$mount('#app1')

Vue.set(app.data, 'b', 2)
```
output : error

Vue 不允許動態添加[根級響應式屬性](https://cn.vuejs.org/v2/guide/reactivity.html)。
> 不能在直接data上增加屬性，可以在data裡的對象上增加屬性。
例如:Vue.set(vm.someobj, {a,"b"})

```js
var app = new Vue({
    el:"#app-demo",
    data:{
        msg:{
            age:18
        }
    }
})
Vue.set(app.msg,"name","jacobhsu");
```