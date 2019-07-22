---
title: Error 
---

vuex 中操作数组，报错 [Do not mutate vuex store state outside mutation handlers](https://www.jianshu.com/p/09327119b009)  

> vuex不允许直接改变state中的东西，必须通过mutations！！

操作是这样，每次赋值新数组（selection）给temp,然后actions中commit motations改变state值currentseletedRows就报这个错。  

`Array.prototype.shift()` - [JavaScript | MDN - Mozilla](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)  
`shift()` 方法會移除並回傳陣列的第一個元素。此方法會改變陣列的長度。