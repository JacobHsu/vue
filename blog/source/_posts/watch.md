---
title: Watch
---

`Watch` 可讓我們監視某個值，當這個值變動的時候，就去做某些事情。

例，如果 userName 發生變化，就去執行 _valid 這個 method。使用者輸入了合法的字串。

https://jsbin.com/zesijokaju/edit?html,output

```html
 <div id="app">
    <p>請輸入使用者名稱。</p>
    <input type="text" v-model="userName">
    <p>${ errMsg }</p>
  </div>
```

```js
<script>
    var vm = new Vue({
    el: '#app',
    delimiters: ['${', '}'],
    data: {
    userName: '',
    errMsg: ''
    },
    watch: {
    userName: function(value) {
        if(this._valid(value)) {
        this.errMsg = '使用者名稱開頭不可為數字。'
        } else {
        this.errMsg = '合法的使用者名稱。';
        }
    }
    },
    methods: {
    _valid: function(name) {
        return /^[0-9]/.test(name);
    }
    }
});
</script>
```

Vue 提供了一種更通用的方式來觀察和響應 Vue 實例上的數據變動：`偵聽屬性`。當你有一些數據需要隨著其它數據變動而變動時，你很容易濫用 watch——特別是如果你之前使用過 AngularJS。然而，通常更好的做法是使用`計算屬性`而不是命令式的 watch 回調。

雖然計算屬性在大多數情況下更合適，但有時也需要一個自定義的偵聽器。這就是為什麼 Vue 通過 watch 選項提供了一個更通用的方法，來響應數據的變化。當需要**在數據變化時執行異步或開銷較大的操作**時，這個方式是最有用的。


#### References

[Vue.js: Watch](https://cythilya.github.io/2017/04/15/vue-watch/)  
[Computed Properties and Watchers](https://vuejs.org/v2/guide/computed.html)  