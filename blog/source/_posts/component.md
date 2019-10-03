---
title: 组件（component）的使用 
---

vue 提供了组件功能，组件又可以分为全局组件和非全局组件。区别是全局组件你可以直接在 .vue 文件中直接使用自定义的 html 即可。非全局组件必须在 Vue 的对象中定义 components 引入这个组件

局部组件引用方式
```js
import A from '@/component/A'
export default {
    data () {},
    components: { A }
}
```


全局组件引用方式
```js
// index.js 文件
import A from '@/component/A'
A.install = function (Vue) {
  Vue.component(A.name, A)
}
export {
    A
}
// main.js 文件
import { A } from './components/index'
Vue.use(A)
```


引入全局組件有一個優化小技巧，上面的方式引入全局組件需要同時維護 index.js 文件和 main.js 文件很麻煩。
採用下面InstallAll的代碼可以只維護 index.js 文件即可

```js
// index.js 文件
import A from '@/component/A'
A.install = function (Vue) {
  Vue.component(A.name, A)
}
//將 main.js Vue.use 改集中放置 index.js   
function InstallAll(Vue) {
    Vue.use(A)
}
export {
    A,
    InstallAll
}
// main.js 文件
import { InstallAll } from './components/index'
InstallAll(Vue)
```

參考：https://juejin.im/post/5d8f0475f265da5b9d1ee503