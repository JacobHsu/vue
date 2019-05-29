---
title: Event Handling
---

[事件修饰符](https://cn.vuejs.org/v2/guide/events.html)  

```html
<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
```

[假設我們今天有一顆按鈕，但是我不希望他就這樣直接submit出去，那怎麼辦？](https://nowills.blogspot.com/2016/08/vue-js-vuejsv-on.html)  
我還是希望他按下按鈕之後會跑出“here I am！”，但是他不要送出資訊轉到submit.html，那這時我們要怎麼做？
很簡單，我們只要寫`v-on:submit.prevent`就好了  