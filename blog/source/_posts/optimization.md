---
title: Optimization
---


### 公用CDN
使用公用的CDN資源，可以起到緩存作用，並減少打包體積

### 使用v-if代替v-show

v-if不渲染DOM，v-show會預渲染DOM
需要頻繁切換顯示狀態 用  `v-if`

### v-for必須加上key，並避免同時使用v-if

為了避免渲染本應該被隱藏的列表 比如 `v-for="user in users" v-if="shouldShowUsers"`。
這種情形下，請將 v-if 移動至容器元素上 (比如 ul, ol)

### 事件及時銷毀

Vue組件銷毀時，會自動清理它與其它實例的連接，解綁它的全部指令及事件監聽器，但是僅限於組件本身的事件。
也就是說，在js內使用addEventListener等方式是不會自動銷毀的，我們需要在組件銷毀時手動移除這些事件的監聽，以免造成內存洩露，如：
```js
created() {
  addEventListener('touchmove', this.touchmove, false)
},
beforeDestroy() {
  removeEventListener('touchmove', this.touchmove, false)
}
```

## 資源提前請求

Vue項目中各文件的加載順序為：router.js、main.js、App.vue、[page].vue
其中，router的加載時間相比於page.vue快近100ms，如果page.vue的文件較多，時間差異會更大 
所以，可以在頁面掛載、渲染的同時去請求接口數據，如在router.js中請求數據：
```
import Router from 'vue-router'
import store from './store'

store.dispatch('initAjax')
```


## 異步路由

使用異步路由可以根據URL自動加載所需頁面的資源，並且不會造成頁面阻塞，較適用於移動端頁面
建議主頁面直接import，非主頁面使用異步路由
```js
{
  path: '/order',
  component: () => import('./views/order.vue')
}
```

## 異步組件

不需要首屏加載的組件都使用異步組件的方式來加載（如多tab），包括需要觸發條件的動作也使用異步組件（如彈窗） 使用方式為：v-if來控制顯示時機，引入組件的Promise即可
```js
<template>
  <div>
    <HellowWorld v-if="showHello" />
  </div>
</template>
<script>
export default {
  components: { HellowWorld: () => import('../components/HelloWorld.vue') },
  data() {
    return {
      showHello: false
    }
  },
  methods: {
    initAsync() {
      addEventListener('scroll', (e) => {
        if (scrollY > 100) {
          this.showHello = true
        }
      })
    }
  }
}
</script>
```

## 異步插件
首屏用不到的插件、或只在特定場景才會用到的插件使用異步加載（如定位插件，部分情況可以通過URL傳遞經緯度；或生成畫報插件，需要在點擊時觸發）；插件第一次加載後緩存在本地，使用方式為：

```js
// 以定位插件為例
const latitude = getUrlParam('latitude')
const longitude = getUrlParam('longitude')
// 如果沒有經緯度參數，則使用定位插件來獲取經緯度
if (!latitude || !longitude) {
  // 首次加載定位插件
  // webpack4寫法，若使用webpack3及以下，則await import('locationPlugin')即可
  if (!this.WhereAmI) this.WhereAmI = (await import('locationPlugin')).default
  // do sth...
}
```

### 減少網路請求
瀏覽器對同一時間針對同一域名下的請求有一定數量限制（一般是6個），超過限制數目的請求會被阻塞

### 打包優化　升級Vue-Cli-3
vue-cli-3採用webpack4+babel7，對編譯打包方面做了很多優化（成倍的提升），使用yarn作為包管理工具，並且對很多優化的最佳實踐做了默認配置

### SSR (Server Side Rendering)

對加載性能要求較高的項目建議升級SSR
> SSR就是因為要解決SPA的SEO問題而出現的解法。 第一個頁面由Server side render，之後的操作還是由Client side render

ref：https://juejin.im/post/5c4e6d7951882522c03ea809