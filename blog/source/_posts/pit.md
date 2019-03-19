---
title: vue的坑
---

##  Vue 不能檢測對象屬性的添加或刪除


https://jsbin.com/mayehetobu/1/edit?html,console,output
```js
<!DOCTYPE html>
<html lang="en-US">
<head>
  <title>My Vue.js App</title>
  <script src="https://cdn.jsdelivr.net/vue/latest/vue.js"></script>
</head>
<body>

<div id="app">
    {{ message }}
</div>

<script type="text/javascript">
    new Vue({
      el: '#app',
      data: {
        message: ''
      }
    });
    // `vm.message` 是響應的
    // 之後設置 `message`
    vm.message = 'Hello!'
    // `vm.message` 是非響應的
</script>

</body>
</html>
```

Vue 不允許在已經創建的實例上動態添加新的根級響應式屬性 (root-level reactive property)。
然而它可以使用 `Vue.set(object, key, value)` 方法將響應屬性添加到嵌套的對象上：


https://jsbin.com/nujomojovo/edit?html,output
```js
<div id="app">
    <p>{{obj}}</p>
</div>
<script>
  var vm = new Vue({
    el: '#app',
    data: {
      obj: {},
    },
    mounted() {
      //想要動態添加對象的屬性並且頁面能更新到
      this.obj.a = 1111  //瀏覽器什麼都沒渲染出來
      this.$set(this.obj,'a',1) // 方法一 { "a": 1 }
      this.obj = Object.assign({}, this.obj,{a:2})  //方法二 可以添加多個屬性 //{ "a": 2 }
    }
  })
</script>
```
https://juejin.im/post/5aa89ab9f265da238b7db328  


https://jsbin.com/mayehetobu/1/edit?html,console,output

```js
<div id="app">
    {{ message }}
     dog {{ counter.dog }}, cat {{ counter.cat }}, penguin {{ counter.penguin }}
</div>

<script type="text/javascript">
    new Vue({
      el: '#app',
      data: {
        message: '',
        counter: {
            // 一開始沒有註冊 penguine，只註冊了 dog 和 cat
            dog: 0,
            cat: 0
        }
      },
      created() {
        // 在 created 的時候才建立 penguine 順便設值為 0
        // 使用 $set 動態新增響應式組件
        this.$set(this.counter, "penguin", 1);
      },
      updated() {
        // 讓我們可以知道組件有被更新
        console.log("view updated");
      }
    });
    //之後設置 `message`
    vm.message = 'Hello!';

</script>
```


物件部分：[一開始沒有被註冊到的物件不會響應式更新](https://pjchender.blogspot.com/2017/05/vue-vue-reactivity.html)  


ref: [深入響應式原理](https://cn.vuejs.org/v2/guide/reactivity.html)  

是其非侵入性的響應式系統。數據模型僅僅是普通的 JavaScript 對象。  
而當你修改它們時，視圖會進行更新。這使得狀態管理非常簡單直接  



![Alt reactivity](https://cn.vuejs.org/images/data.png)