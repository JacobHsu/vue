---
title: keep-alive
---

在使用`vue + vue-router`開發SPA的時候，有沒有遇到過這樣的情況：當我們在列表頁和詳情頁之間切換的時候，如果列表頁不做緩存，會導致每次從詳情頁返回時，列表頁都會重新加載。

### keep-alive實現頁面緩存

#### 方法一：
```js
routes:[{
    path: '/search',
    name: 'search',
    component: search,
    meta: {
        title: '搜索列表頁',
        keepAlive: true // 標記列表頁需要被緩存
    }
},
{
    path: '/detail',
    name: 'detail',
    component: detail,
    meta: {
        title: '詳情頁',
        // 詳情頁不需要做緩存，所以不加keepAlive標記
    }
}]
```

由於`<keep-alive>`組件不支持v-if指令，所以我們在App.vue中採用兩個`<router-view>`的寫法，通過當前路由的keepAlive字段來判斷是否對頁面進行緩存：  

```js
<div id="app">
    <keep-alive>
        <router-view v-if="$route.meta.keepAlive" />
    </keep-alive>
    <router-view v-if="!$route.meta.keepAlive" />
</div>
```

#### 方法二

使用`<keep-alive>`提供的 `exclude` 或者 `include` 選項，
此處我們使用 `exclude` ，在App.vue中：

```js
<div id="app">
    <keep-alive exclude="detail">
        <router-view />
    </keep-alive>
</div>
```

這麼寫就代表了在項目中除了name為detail的頁面組件外(`exclude`)，其餘頁面都將進行緩存。  


# References    

[keep-alive + vuex 让缓存的页面灵活起来](https://juejin.im/post/5cb060b2e51d456e7618a69a?utm_source=gold_browser_extension)  
 