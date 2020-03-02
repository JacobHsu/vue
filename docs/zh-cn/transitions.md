# 进入/离开 & 列表过渡

## 概述

Vue 在插入、更新或者移除 DOM 时，提供多种不同方式的应用过渡效果。
包括以下工具：

* 在 CSS 过渡和动画中自动应用 class
* 可以配合使用第三方 CSS 动画库，如 Animate.css
* 在过渡钩子函数中使用 JavaScript 直接操作 DOM
* 可以配合使用第三方 JavaScript 动画库，如 Velocity.js
* 在这里，我们只会讲到进入、离开和列表的过渡

## 单元素/组件的过渡

`Vue` 提供了 `transition` 的封装组件，在下列情形中，可以给任何元素和组件添加进入/离开过渡

条件渲染 (使用 `v-if`)
条件展示 (使用 `v-show`)
动态组件
组件根节点

这里是一个典型的例子：


<iframe width="100%" height="300" src="//jsfiddle.net/JacobHsu/nomeqfxh/1/embedded/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

```html
<div id="demo">
  <button v-on:click="show = !show">
    Toggle
  </button>
  <transition name="fade">
    <p v-if="show">hello</p>
  </transition>
</div>
```

```css
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s; /* 时刻监听opacity的属性 一旦发生变化 .5秒内发生过渡 */
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0; /* 被移除默认值为1 不透明(显示) */
}
```

![alt hello](https://i.imgur.com/ZsMEqZl.png?1 "单元素/组件的过渡")

CSS（层叠样式表）[opacity](https://developer.mozilla.org/zh-CN/docs/Web/CSS/opacity)

默认前缀 `v-` (no name)  

```html
  <transition>
    <p v-if="show">hello</p>
  </transition>
```

```css
.v-enter-active, .v-leave-active {
  transition: opacity .5s;
}
.v-enter, .v-leave-to /* .v-leave-active below version 2.1.8 */ {
  opacity: 0;
}
```
