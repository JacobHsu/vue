---
title: Different 
---

[vuejs v-text 和 v-html的区别](https://www.cnblogs.com/jinbang/p/6790592.html)


`{{message}}`：將數據解析為純文本，不能輸出真正的html，在頁面加載時顯示花括號，所以通常使用v-html和v-text代替，且花括號方式在以後可能被取消

`v-html="html"`：輸出真正的html

`v-text="text"`：將數據解析為純文本，不能輸出真正的html，與花括號的區別是在頁面加載時不顯示花括號

https://jsbin.com/wadoyiqabo/edit?html,output
```html
<div id="app">
　　<p>{{message}}</p> <!-- 輸出：<span>通過雙括號綁定</span> -->
　　<p v-html="html"></p> <!-- 輸出：html標籤在渲染的時候被解析 -->
　　<p v-text="text"></p> <!-- 輸出：<span>html標籤在渲染的時候被源碼輸出</span> -->
</div>


<script>
　　let app = new Vue({
　　el: "#app",
　　data: {
　　　　message: "<span>通過雙括號綁定</span>",
　　　　html: "<span>html標籤在渲染的時候被解析</span>",
　　　　text: "<span>html標籤在渲染的時候被源碼輸出</span>",
　　}
});
</script>

```