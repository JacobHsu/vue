---
title: Vue與React
---


最大的變化是React推廣了Virtual DOM並創造了新的語法——JSX，`JSX`允許開發者在JavaScript中書寫HTML（即`HTML in JavaScript`）。

Vue致力解決的問題與React一致，但卻提供了另外一套解決方案。Vue使用`模板系統`而不是JSX，使其對現有應用的升級更加容易。這是因為模板用的就是`普通的HTML`，通過Vue來整合現有的系統是比較容易的

#### 相似之處

都是JavaScript的UI框架，專注於創造前端的富應用  
Reat與Vue只有框架的骨架，其他的功能如路由、狀態管理等是框架分離的組件。  

相似但略有不同之處是它們配套框架的處理方法。
相同之處在於，兩個框架都專注於UI層，其他的功能如路由、狀態管理等都交由同伴框架進行處理。
而不同之處是在於它們如何關聯它們各自的配套框架。
Vue的核心團隊維護著`vue-router`和`vuex`，它們都是作為官方推薦的存在。  
而React的`react-router`和`react-redux`則是由社區成員維護，它們都不是官方維護的。  

### Virtual DOM
Vue與React的其中最大一個相似之處，就是他們都使用了`Virtual DOM`。
虛擬DOM，DOM樹的虛擬表現。它的誕生是基於這麼一個概念：改變真實的DOM狀態遠比改變一個JavaScript對象的花銷要大得多

Virtual DOM是一個映射真實DOM的JavaScript對象，如果需要改變任何元素的狀態，那麼是先在Virtual DOM上進行改變，而不是直接改變真實的DOM。當有變化產生時，一個新的Virtual DOM對象會被創建並**計算新舊Virtual DOM之間的差別**。之後這些差別會應用在真實的DOM上。

**計算差異的算法是高性能框架的秘密所在，React和Vue在實現上有點不同。**

Vue宣稱可以更快地計算出Virtual DOM的差異，這是由於它在渲染過程中，會跟蹤每一個組件的依賴關係，**不需要重新渲染整個組件樹**。

而對於React而言，**每當應用的狀態被改變時，全部子組件都會重新渲染**。當然，這可以通過shouldComponentUpdate這個生命週期方法來進行控制，但Vue將此視為默認的優化。

### 組件化

React與Vue都鼓勵組件化應用。這本質上說，是建議你將你的應用分拆成一個個功能明確的模塊，每個模塊之間可以通過合適的方式互相聯繫。

在Vue中，如果你遵守一定的規則，你可以使用單文件組件。  
HTML, JavaScript和CSS都寫在一個文件之中。你不再需要在.vue組件文件中引入CSS，雖然這也是可以的。  
React也是非常相似的，JavaScript與JSX被寫入同一個組件文件中。  

### Props

我們可以看到React和Vue都有`props`的概念，這是`properties`的簡寫。
props在組件中是一個特殊的屬性，**允許父組件往子組件傳送數據**。  

```js
Object.keys(this.state.pastadishes).map(key =>
    <PastaItem index={key} key={key} details={this.state.pastadishes[key]} addToOrder={this.addToOrder} orders={this.state.orders[key]} />
)
```

上面的JSX庫組中，index, key, details, orders 與 addToOrder都是`props`，數據會被下傳到子組件`PastaItem`中去。
在React中，這是必須的，它依賴一個`單一數據源`作為它的`狀態`  


而在Vue中，props略有不同。它們一樣是在組件中被定義，但Vue依賴於模板語法，你可以通過模板的循環函數更高效地展示傳入的數據。
```js
<pasta-item v-for="(item, key) in samplePasta" :item="item" :key="key" @order="handleOrder(key)"></pasta-item>
```
這是模板的實現，但這代碼完全能工作，然而在React中展現相同數據會更麻煩一點。  

### 模板 vs JSX

React與Vue最大的不同是模板的編寫。
Vue鼓勵你去寫近似常規HTML的模板。寫起來很接近標準HTML元素，只是多了一些屬性。
Vue鼓勵你去使用HTML模板去進行渲染，使用相似於Angular風格的方法去輸出動態的內容。因此，通過把原有的模板整合成新的Vue模板，Vue很容易提供舊的應用的升級。這也讓新來者很容易適應它的語法。  

另一方面，React推薦你所有的模板通用JavaScript的語法擴展——JSX書寫。

```js
<ul>
  <pasta-item v-for="(item, key) in samplePasta" :item="item" :key="key" @order="handleOrder(key)"></pasta-item>
</ul>
```

```js
<ul className="pasta-list">
    {
        Object.keys(this.state.pastadishes).map(key =>
            <PastaItem index={key} key={key} details={this.state.pastadishes[key]} addToOrder={this.addToOrder} orders={this.state.orders[key]} />
        )
    }
</ul>
```
React/JSX乍看之下，覺得非常囉嗦，但使用JavaScript而不是模板來開發，賦予了開發者許多編程能力。
JSX只是JavaScript混合著XML語法，然而一旦你掌握了它，它使用起來會讓你感到暢快。  
而相反的觀點是Vue的模板語法去除了往視圖/組件中添加邏輯的誘惑，保持了關注點分離。  


### 狀態管理 vs 對象屬性

狀態是React關鍵的概念。也有一些配套框架被設計為管理一個大的state對象，如Redux。此外，state對象在React應用中是不可變的，意味著它不能被直接改變。在React中你需要使用`setState()`方法去更新狀態。
在Vue中，state對象並不是必須的，數據由data屬性在Vue對象中進行管理。  
而在Vue中，則不需要使用如setState()之類的方法去改變它的狀態，在Vue對象中，data參數就是應用中數據的保存者。

對於管理大型應用中的狀態這一話題而言，Vue.js的作者尤雨溪曾說過，Vue解決方案適用於小型應用，但對於對於大型應用而言不太適合。
多數情況下，框架內置的狀態管理是不足以支撐大型應用的，Redux或Vuex等狀態管理方案是必須使用的。



### References  
https://deliciousbrains.com/vue-vs-react-battle-javascript