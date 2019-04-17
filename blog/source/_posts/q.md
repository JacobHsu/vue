---
title: Question
---

### v-show 與 v-if 區別

`v-show`是css切換，`v-if`是完整的銷毀和重新創建。
v-if=‘false’ v-if是條件渲染，當false的時候不會渲染
使用 頻繁切換時用`v-show`，運行時較少改變時用v-if


### 計算屬性(computed)和 watch 的區別

區別來源於用法，只是需要動態值，那就用計算屬性；需要知道值的改變後執行業務邏輯，才用 watch  
當有一些數據需要隨著另外一些數據變化時，建議使用computed。   
當有一個通用的響應數據變化的時候，要執行一些業務邏輯或異步操作的時候建議使用watcher  

computed 是一個對象時，有get和set兩個選項
computed 和 methods 有什麼區別？ methods是一個方法，它可以接受參數，而computed不能，computed是可以緩存的，methods不會。
computed 是否能依賴其它組件的數據？ computed可以依賴其他computed，甚至是其他組件的data

### 組件中 data 為什麼是函數

為什麼組件中的 data 必須是一個函數，然後 return 一個對象，而 new Vue 實例裡，data 可以直接是一個對象？

因為**組件是用來復用的**，JS 裡對象是引用關係，這樣作用域沒有隔離，而 new Vue 的實例，是不會被復用的，因此不存在引用對象的問題。

### 在動態組件上使用 keep-alive  

多標籤的界面中使用 is 特性來切換不同的組件

當在這些組件之間切換的時候，你有時會想保持這些組件的狀態，以**避免反覆重渲染導致的性能問題**

我們更希望那些標籤的組件實例能夠被在它們**第一次被創建的時候緩存下來**。為瞭解決這個問題，我們可以用一個 <keep-alive> 元素將其動態組件包裹起來。

```js
<!-- 失活的組件將會被緩存！-->
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```

https://cn.vuejs.org/v2/guide/components-dynamic-async.html#在动态组件上使用-keep-alive


### Render函數 怎樣理解單向數據流 

這個概念出現在組件通信。
父組件是通過 `prop` 把數據傳遞到子組件的，但是這個 prop 只能由父組件修改，子組件不能修改，否則會報錯。
子組件想修改時，只能通過` $emit` 派發一個自定義事件，父組件接收到後，由父組件修改。
一般來說，對於子組件想要更改父組件狀態的場景，可以有兩種方案：
在子組件的 data 中拷貝一份 prop，data 是可以修改的，但 prop 不能

### 生命週期

*創建前後* beforeCreate/created
在beforeCreate 階段，vue實例的掛載元素el和數據對象data都為undefined，還未初始化。
在created階段，vue實例的數據對象有了，el還沒有。

*載入前後* beforeMount/mounted
在beforeMount階段，vue實例的$el和data都初始化了，但還是掛載之前未虛擬的DOM節點，data尚未替換。
在mounted階段，vue實例掛載完成，data成功渲染。

*更新前後* beforeUpdate/updated
當data變化時，會觸發beforeUpdate和updated方法。這兩個不常用，不推薦使用。

*銷毀前後* beforeDestory/destoryed
beforeDestory是在vue實例銷毀前觸發，一般在這裡要通過removeEventListener**解除手動綁定的事件**。
實例銷毀後，觸發的destroyed。

### 組件間的通信

父子 props/event  parent/children ref provide/inject
兄弟 bus vuex
跨級 bus vuex provide.inject

### 路由的跳轉方式

<router-link to='home'> router-link標籤會渲染為<a>標籤，template中的跳轉都是這種  
另一種是編程是導航 也就是通過`js跳轉` 比如 `router.push('/home')`  


### Vue.js 雙向綁定原理

核心的 API 是通過 Object.defineProperty() 來劫持各個屬性的setter / getter，在數據變動時發佈消息給訂閱者，觸發相應的監聽回調
https://cn.vuejs.org/v2/guide/reactivity.html



### Vuex與Redux的主要區別在哪裡，兩者各有什麼優缺點？

Vuex 其實是一個針對 Vue **特化的 Flux**，主要是為了配合 Vue 本身的響應式機制。當然吸取了一些 Redux 的特點，
比如單狀態樹和便於測試和熱重載的 API，但是也選擇性的放棄了一些在 Vue 的場景下並不契合的特性，
比如強制的 immutability（在保證了每一次狀態變化都能追蹤的情況下強制的 immutability 帶來的收益就很有限了）、為了同構而設計得較為繁瑣的 API、必須依賴第三方庫才能相對高效率地獲得狀態樹的局部狀態等等（相比之下 Vuex 直接用 Vue 本身的計算屬性就可以）

所以 `Vue + Vuex` 會更簡潔，也不需要考慮性能問題，代價就是 Vuex 只能和 Vue 配合。
Vue + Redux 也不是不可以，但是 Redux 作為一個泛用的實現和 Vue 的契合度肯定不如 Vuex。

作者：尤雨溪
链接：https://www.zhihu.com/question/38546875/answer/76970954
来源：知乎

#### Flux

FLUX是一個由Facebook提出來的開發架構(FLUX 是一個 Pattern 而不是一個正式的框架)，目的是在解決所謂的MVC在大型商業網站所存在的問題，把沒有條理跟亂七八糟的架構做一個流程規範的定義。

FLUX的中心思想打造**單一的資料流進行方式(one-way data flow)**，相對於MVC只定義了三個角色的功能與關係，FLUX更明確的定義了一個資料進行的方式，使得大家更容易遵守規則。

https://dotblogs.com.tw/blackie1019/2015/04/14/151049

#### Immutable

Immutable 翻成中文就是永遠不變的，意思就是：「當一個資料被創建之後，就永遠不會變了」。那如果我需要更改資料的話怎麼辦呢？你就只能創一個新的。

```js
const obj = {
  id: 1,
  text: 'hello'
}
  
obj.text = 'world' // 這樣不行，因為你改變了 obj 這個物件
  
// 你必須要像這樣創造一個新的物件
const newObj = {
  ...obj,
  text: 'world'
}
```

這也是為什麼我們在用setState的時候總是要產生一個新的物件，而不是直接對現有的做操作。
還有一個要注意的地方，那就是 spread operator 只會複製第一層的資料而已，它並不是 deep clone  

在學習 React 的時候搞懂 Immutable 是很重要的，它可以讓你之後的程式減少互相干擾的機會，讓你除錯的時候邏輯清晰。

https://blog.techbridge.cc/2018/01/05/react-render-optimization/  

### nextTick()

Vue中的nextTick涉及到**Vue中DOM的異步更新**

[Vue 實現響應式](https://cn.vuejs.org/v2/guide/reactivity.html)並不是數據發生變化之後 DOM 立即變化，而是按一定的策略進行 DOM 的更新。(異步更新隊列)  
在下次 DOM 更新循環結束之後執行延遲回調。在修改數據之後，立即使用這個回調函數，獲取更新後的 DOM。
```js
<div id="example">{{message}}</div>
var vm = new Vue({
  el: '#example',
  data: {
    message: '123'
  }
})
vm.message = 'new message' // 更改數據
vm.$el.textContent === 'new message' // false
Vue.nextTick(function () {
  vm.$el.textContent === 'new message' // true
})
```

https://jsbin.com/xafilatoka/edit?html,output

看完這個示例，也許有人會問，我在 Vue 實例方法中修改了數據，
然後再在 $nextTick 回調中獲取該數據在相應 DOM 元素所綁定的內容（或屬性）殊無必要，我為什麼需要這樣的 API 呢？

考慮這樣一種場景，**你有一個 jQuery 插件，希望在 DOM 元素中某些屬性發生變化之後重新應用該插件，這時候就需要在 $nextTick 的回調函數中執行重新應用插件的方法。**


```js
// 修改數據
vm.msg = 'Hello'
// DOM 還未更新
Vue.nextTick(function () {
  // DOM 更新
})
```
其根本原因是因為Vue中DOM更新是異步的

在Vue生命週期的`created()`鉤子函數進行的DOM操作一定要放在`Vue.nextTick()`的回調函數中

在created()鉤子函數執行的時候DOM 其實並未進行任何渲染，而此時進行DOM操作無異於徒勞，
所以此處一定要將DOM操作的js代碼放進`Vue.nextTick()`的回調函數中。

與之對應的就是mounted()鉤子函數，因為該鉤子函數執行時所有的DOM掛載和渲染都已完成，
此時在該鉤子函數中進行任何DOM操作都不會有問題 。

在數據變化後要執行的某個操作，而這個操作需要使用隨數據改變而改變的DOM結構的時候，這個操作都應該放進Vue.nextTick()的回調函數中。





https://juejin.im/post/5c97002b6fb9a070aa5cf60b
https://zhuanlan.zhihu.com/p/26724001
