---
title: Vue Router
---

Vue Router: [介绍](https://router.vuejs.org/zh/)  

## vue-router的 `hash` 和 `history` 两种模式的区别  

#### 為什麼要有 hash 和 history

對於 Vue 這類漸進式前端開發框架，為了構建 **SPA（單頁面應用）**，需要引入前端路由系統，這也就是 Vue-Router 存在的意義。前端路由的核心，就在於 —— **改變視圖的同時不會向後端發出請求**。

為了達到這一目的，瀏覽器當前提供了以下兩種支持：

`hash` —— 即地址欄 URL 中的 `#` 符號。比如這個 URL：http://www.abc.com/#/hello，hash 的值為 #/hello。它的特點在於：hash 雖然出現在 URL 中，但不會被包括在 HTTP 請求中，對後端完全沒有影響，因此改變 hash 不會重新加載頁面。  


`history` —— 利用了 HTML5 History Interface 中新增的 pushState() 和 replaceState() 方法。（需要特定瀏覽器支持）這兩個方法應用於瀏覽器的歷史記錄棧，在當前已有的 `back`、`forward`、`go` 的基礎之上，它們提供了對歷史記錄進行修改的功能。只是當它們執行修改時，雖然改變了當前的 URL，但瀏覽器不會立即向後端發送請求。


因此可以說，hash 模式和 history 模式都屬於瀏覽器自身的特性，Vue-Router 只是利用了這兩個特性（通過調用瀏覽器提供的接口）來實現前端路由。

[HTML5 History 模式](https://router.vuejs.org/zh/guide/essentials/history-mode.html) 
> 如果不想要很醜的 hash，我們可以用路由的 history 模式，這種模式充分利用 `history.pushState API` 來完成 URL 跳轉而無須重新加載頁面。

```js
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
```

`history` 模式下，前端的 URL 必須和實際向後端發起請求的 URL 一致，如 `http://www.abc.com/book/id`。如果後端缺少對 `/book/id` 的路由處理，將返回 `404` 錯誤。

Vue-Router 官網裡如此描述：
>不過這種模式要玩好，還需要後台配置支持……所以呢，你要在服務端增加一個覆蓋所有情況的候選資源：如果 URL 匹配不到任何靜態資源，則應該返回同一個 index.html 頁面，這個頁面就是你 app 依賴的頁面。

### 小結

結合自身例子，對於一般的 `Vue + Vue-Router + Webpack + XXX` 形式的 Web 開發場景，用 `history` 模式即可，只需在後端（`Apache 或 Nginx`）進行簡單的路由配置，同時搭配前端路由的 404 頁面支持。
