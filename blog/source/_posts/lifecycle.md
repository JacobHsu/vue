---
title: Vue生命週期
---

Vue執行了一連串工作，包含在原始資料加料、建立Vue Instance、編譯樣板、綁定資料等，隨著資料新增修改，週而復始直到刪除，稱為生命週期(Lifecycle)。

從開始創建、初始化數據、編譯模板、掛載Dom→渲染、更新→渲染、卸載等一系列過程

在他做這些工作的時機點前後，提供你客製化的空間，稱為`Hook`(如圖上白底紅字的部分)。

![lifecycle](https://cn.vuejs.org/images/lifecycle.png)

| 生命週期鉤子   |     組件狀態      |  最佳實踐 |
|----------|:-------------:|------:|
| beforeCreate |  實例初始化之後，數據與事件配置之前 | 初始化非響應式變量 |
| created | 已將data和methods掛載vue到實例上，未掛載到DOM | 可初始化ajax請求，頁面的初始化 |
| beforeMount  | 模板編譯完成，但未掛載，無法讀取DOM |     |
| mounted  | 實例掛載到DOM上，此時可以通過DOM API獲取到DOM節點 |  常用於獲取VNode信息和操作，ajax請求   |
| beforeupdate  | 響應式數據更新時調用 |  適合在更新之前訪問現有的DOM，比如手動移除已添加的事件監聽器   | 
| updated  | 當組件響應式變量變更時調用，可執行依賴於DOM的操作 |  避免在這個鉤子函數中操作數據，可能陷入死循環   | 
| beforeDestroy  | 實例銷毀之前調用 | 常用於銷毀定時器、解綁全局事件、銷毀插件對象等操作    |
| destroyed  | 實例銷毀後調用，調用後，Vue 實例指示的所有東西都會解綁定，所有的事件監聽器會被移除，所有的子實例也會被銷毀 |     |

資料在created以後才存取得到(意味別把資料初始化跟ajax寫在beforeCreate)  
created階段的ajax請求與mounted請求的區別：  
前者頁面視圖未出現，如果請求信息過多，頁面會長時間處於白屏狀態  
如果要對 DOM 做動作，記得要在 mounted 掛鉤中，它如同 onload 事件，或是 jQuery 中的 ready()  

| 生命週期鉤子   |     組件狀態      | 
|----------|:-------------:|------:|
| beforeCreate |  不能訪問到data、computed、watch、methods上的方法和數據 |  |
| created| 可訪問方法和數據，不能訪問到$el屬性，$ref屬性內容為空數組  |    |
| beforeMount  | 在掛載開始之前被調用`，找到對應的template，並編譯成render函數 |     |
| mounted  | $ref屬性可以訪問 |     |
| beforeupdate  | 發生在虛擬DOM打補丁之前 |     |
| updated  | 虛擬 DOM 重新渲染和打補丁之後調用 |     |
| beforeDestroy  | 這一步，實例仍然完全可用，this仍能獲取到實例 |     |
| destroyed  | 所有的子實例會被銷毀 |     |

初始化組件時，僅執行了beforeCreate/Created/beforeMount/mounted四個鉤子函數

當改變data中定義的變量（響應式變量）時，會執行beforeUpdate/updated鉤子函數

當切換組件（當前組件未緩存）時，會執行beforeDestory/destroyed鉤子函數

初始化和銷毀時的生命鉤子函數均只會執行一次，  beforeUpdate/updated可多次執行


https://jsbin.com/melanen/edit?html,js,console,output

```js
new Vue({
  el: '#app',
  data: {
    a: 1
  }
})

== beforeCreate ==
this.a: undefined
this.$el: undefined

== created ==
this.a: 1
this.$el: undefined

== mounted ==
this.a: 1
this.$el: [object HTMLDivElement]
```

`beforeCreate`時，資料和元素掛載都還沒被建立  
`created`時，資料已經可以取得了，但元素掛載還沒   
`mounted`時，全部都被建立了，如果你想改變 DOM 或資料，可以在這裡做，因為它還沒被瀏覽器「畫」出來，必須直到   
`updated` 執行時，才會把最終的結果畫在瀏覽器上被看到。

### 父子組件的生命週期

1.僅當子組件完成掛載後，父組件才會掛載  
2.當子組件完成掛載後，父組件會主動執行一次  3.beforeUpdate/updated鉤子函數（僅首次）  
4.父子組件在data變化中是分別監控的，但是在更新props中的數據是關聯的（可實踐）  
銷毀父組件時，先將子組件銷毀後才會銷毀父組件  

## References  

[Lifecycle Diagram](https://vuejs.org/v2/guide/instance.html#Lifecycle-Diagram)  
https://juejin.im/entry/5aee8fbb518825671952308c

