---
title: Handling Errors in Vue.js
---

# 三種類型的錯誤

### 引用一個不能存在的變量：

```js
<div id="app" v-cloak>
  Hello, {{name}}
</div>
```
運行後不會拋出錯誤，但是在控制台會有`[Vue warn]`消息

[Codepen](https://codepen.io/cfjedimaster/embed/qweOKB?height=265&theme-id=0&default-tab=js%2Cresult&user=cfjedimaster&slug-hash=qweOKB&pen-title=Error1A&name=cp_embed_1)



### 將變量綁定到一個被計算出來的屬性，計算的時候會拋出異常：

```js
<div id="app" v-cloak>
  Hello, {{name2}}
</div>

<script>
const app = new Vue({
  el:'#app',
  computed:{
    name2() {
      return x;
    }
  }
})
</script>
```
[Codepen](https://codepen.io/cfjedimaster/embed/BEXoOw?height=265&theme-id=0&default-tab=js%2Cresult&user=cfjedimaster&slug-hash=BEXoOw&pen-title=Error1B&name=cp_embed_2)

###　執行一個會拋出異常的方法

```html
<div id="app" v-cloak>
	<button @click="doIt">Do It</button>
</div>

<script>
const app = new Vue({
  el:'#app',
  methods:{
	  doIt() {
		  return x;
	  }
  }
})
</script>
```

這個錯誤在控制台也`[Vue warn]`和常規報錯。和計算錯誤的區別在於，只有你點擊了按鈕才會觸發函數調用，才會報錯。

[Codepen](https://codepen.io/cfjedimaster/embed/oOKjJb?height=265&theme-id=0&default-tab=js%2Cresult&user=cfjedimaster&slug-hash=oOKjJb&pen-title=Error1C&name=cp_embed_3) 


> 如果在組件渲染時出現運行錯誤，錯誤將會被傳遞至全局 `Vue.config.errorHandler` 配置函數 (如果已設置)。利用這個鉤子函數來配合錯誤跟蹤服務是個不錯的主意。比如 [Sentry](https://sentry.io/for/vue/)，它為 Vue 提供了官方集成。

# Vue中異常處理包含以下幾個方面的技巧：

[errorHandler](https://vuejs.org/v2/api/#errorHandler)  
[warnHandler](https://vuejs.org/v2/api/#warnHandler) 
> Note this only works during development and is ignored in production.  
renderError  
[errorCaptured](https://link.juejin.im/?target=https%3A%2F%2Fcn.vuejs.org%2Fv2%2Fapi%2F%23errorCaptured)  
[window.onerror](https://developer.mozilla.org/en-US/docs/Web/API/GlobalEventHandlers/onerror) (不僅僅針對Vue)  


### [errorHandler](https://vuejs.org/v2/api/#errorHandler) 

`err`指代error對象，`info`是一個Vue特有的字符串(信息非常有用的)，`vm`指代Vue應用本身。記住在一個頁面你可以有多個Vue應用。這個error handler作用到所有的應用。
```js
Vue.config.errorHandler = function(err, vm, info) {
  console.log(`Error: ${err.toString()}\nInfo: ${info}`);
}
```

1.變量錯誤不會觸發errorHandler，它只是一個warning。
2.計算錯誤會拋出錯誤被errorHandler捕獲：
```js
Error: ReferenceError: x is not defined
Info: render
```
3.方法錯誤也會被捕獲：
```js
Error: ReferenceError: x is not defined
Info: v-on handler
```

### renderError

和前面兩個不同，這個技巧不適用於全局，和組件相關。並且只適用於非生產環境。

[codepen](https://codepen.io/cfjedimaster/embed/NmQrwa?height=265&theme-id=0&default-tab=result&user=cfjedimaster&slug-hash=NmQrwa&pen-title=Error1B%20with%20renderError&name=cp_embed_7)

```js
Vue.config.productionTip = false;
Vue.config.devtools = false;

Vue.config.errorHandler = function(err, vm, info) {
  console.log(`Error: ${err.toString()}\nInfo: ${info}`);
}

Vue.config.warnHandler = function(msg, vm, trace) {
  console.log(`Warn: ${msg}\nTrace: ${trace}`);
}

const app = new Vue({
  el:'#app',
  computed:{
    name2() {
      return x;
    }
  },
  renderError (h, err) {
    return h('pre', { style: { color: 'red' }}, err.stack)
  }
})
```

### errorCaptured

這個error Handler只能在父組件中處理子組件的錯誤。據我所知，我們沒法直接在Vue的主實例(main instance)中使用它。

errorCaptured是個很有趣的特性，我想哪些構建組件庫的開發者應該會用到吧。這個特性更像是一個面向組件開發者而不是一般開發者。

[實例](https://link.juejin.im/?target=https%3A%2F%2Fcodepen.io%2Fcfjedimaster%2Fembed%2FMRMbYJ%3Fheight%3D265%26theme-id%3D0%26default-tab%3Djs%252Cresult%26user%3Dcfjedimaster%26slug-hash%3DMRMbYJ%26pen-title%3DError1%26name%3Dcp_embed_8)

### window.onerror 終極技巧

最後也是最重要的一個候選項 `window.onerror`。
它是一個全局的異常處理函數，可以抓取所有的JavaScript異常。

如果你定義了`onerror`，但是沒有啟用`Vue.config.errorHandler`，那麼有很多異常都抓不到。Vue希望你要定義它，否則異常不會拋出去的

[運行實例](https://codepen.io/cfjedimaster/embed/WWVowN?height=265&theme-id=0&default-tab=js%2Cresult&user=cfjedimaster&slug-hash=WWVowN&pen-title=Error1C%20with%20Handler%20(window))

```js
window.onerror = function(message, source, line, column, error) {
  console.log('ONE ERROR HANDLER TO RULE THEM ALL:', message);
}

Vue.config.productionTip = false;
Vue.config.devtools = false;

Vue.config.errorHandler = function(err, vm, info) {
  //oopsIDidItAgain();
  console.log(`Error: ${err.toString()}\nInfo: ${info}`);
}

const app = new Vue({
  el:'#app',
  methods:{
    doIt() {
      return x;
    }
  }
})
```

#### References

[Handling Errors in Vue.js](https://www.raymondcamden.com/2019/05/01/handling-errors-in-vuejs)