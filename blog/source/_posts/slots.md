---
title: slots 插槽
---

Vue 實現了一套內容分發的 API，將 `<slot>` 元素作為承載分發內容的出口

[slots](https://cn.vuejs.org/v2/guide/components-slots.html)  


## 具名插槽（Named Slots）

使用屬性 name 決定配置的內容。沒有 name 的匿名插槽將成為預設插槽，匹配不到的內容會放置在此；若沒有默認插槽，匹配不到的內容會被捨棄。

https://codepen.io/JacobHsu/pen/zQwMdN

```html
<div id="app">
  <layout>
    <p slot="header">header這裡可能是一個頁面標題</p>
    <p>主要內容的一個段落。</p>
    <p>另一個主要段落。</p>
    <p slot="footer">footer這裡有一些聯繫信息</p>
  </layout>
</div>
```

```js
Vue.component('layout', {
  template: `
    <div class="container">
      <header>
        <slot name="header"></slot>
      </header>
      <main>
        <slot></slot>
      </main>
      <footer>
        <slot name="footer"></slot>
      </footer>
    </div>`
});

var vm = new Vue({
  el: '#app'
});
```

header這裡可能是一個頁面標題  
主要內容的一個段落。  
另一個主要段落。  
footer這裡有一些聯繫信息  

### References

[Vue.js: Slot](https://cythilya.github.io/2017/10/11/vue-component-slot/)  