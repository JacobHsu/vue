---
title: render function
---

Vue 推薦使用在絕大多數情況下使用 template 來創建你的 HTML。然而在一些場景中，你真的需要 JavaScript 的完全編程的能力，這就是 `render 函數`，它比 template 更接近編譯器。  

如果你真的需要重複很多次的元素/組件，你可以使用工廠函數來實現。
例如，下面[這個例子](https://cn.vuejs.org/v2/guide/render-function.html#%E7%BA%A6%E6%9D%9F) render 函數完美有效地渲染了 20 個相同的段落：

https://jsbin.com/lidohak/edit?html,js,output
```js
render: function (createElement) {
  return createElement('div',
    Array.apply(null, { length: 20 }).map(function () {
      return createElement('p', 'hi')
    })
  )
}
```