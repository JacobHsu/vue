# vue

[Vue](https://cn.vuejs.org/v2/guide/) (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，易于上手。Vue 也完全能够为复杂的单页应用提供驱动。

## 声明式渲染

Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统：

```html
<div id="app">
  {{ message }}
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

> Hello Vue!

## Vue CLI

[Vue CLI](https://cli.vuejs.org/zh/guide/) 是一个基于 Vue.js 进行快速开发的完整系统，提供：
* 通过 @vue/cli 搭建交互式的项目脚手架。

创建一个项目 vue create

```sh
vue create hello-world
```

<iframe
    src="https://codesandbox.io/embed/still-darkness-l7id6?fontsize=14&hidenavigation=1&theme=dark"
    style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
    title="vue-cli"
    allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb"
    sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"
></iframe>
