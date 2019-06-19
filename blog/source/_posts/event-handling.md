---
title: Event Handling
---

# [事件修饰符](https://cn.vuejs.org/v2/guide/events.html)  

更好的方式是：方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节。

为了解决这个问题，Vue.js 为 `v-on` 提供了`事件修饰符`。

`.stop` 等同於`event.stopPropagation()`，防止事件冒泡。
`.prevent` 等同於`event.preventDefault()`，防止執行預設的行為。
`.capture` 與事件冒泡的方向相反，事件捕獲 (event capturing) 是由外而內的。
`.self` 只會觸發自己範圍內的事件，不包含子元素。
`.once` 只會觸發一次  
`.passive`  


```html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
```

###　.prevent

使用　`<a href="#">`　當成點擊的按鈕時，通常會使用　`event.preventDefault()`　，防止瀏覽器的網址列出現 `#`。

```html
<div id="app">
  <a href="#" type="button" @click.prevent="prompt">按我取得訊息</a>
  <p>${ msg }</p>
</div>
```

```js
var vm = new Vue({
  el: '#app',
  delimiters: ['${', '}'],
  data: {
    msg: '沒發生任何事情！',
  },
  methods: {
    prompt() {
      this.msg = '按鈕被按了！監聽到 click 事件。';
    }
  }
});
```

[Vue.js: Methods 與事件處理 (Event Handling)](https://cythilya.github.io/2017/04/17/vue-methods-and-event-handling/)  