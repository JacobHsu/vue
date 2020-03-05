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

当插入或删除包含在 `transition` 组件中的元素时，Vue 将会做以下处理：  

1. 自动嗅探目标元素是否应用了 CSS 过渡或动画，如果是，在恰当的时机添加/删除 CSS 类名。

2. 如果过渡组件提供了 [JavaScript 钩子函数]((https://cn.vuejs.org/v2/guide/transitions.html#JavaScript-钩子))，这些钩子函数将在恰当的时机被调用。

3. 如果没有找到 JavaScript 钩子并且也没有检测到 CSS 过渡/动画，DOM 操作 (插入/删除) 在下一帧中立即执行。(注意：此指浏览器逐帧动画机制，和 Vue 的 nextTick 概念不同)

## 过渡的类名

在进入/离开的过渡中，会有 6 个 class 切换。

1. `v-enter`：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。

2. `v-enter-active`：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。

3. `v-enter-to`: 2.1.8版及以上 定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 v-enter 被移除)，在过渡/动画完成之后移除。

4. `v-leave`: 定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。

5. `v-leave-active`：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。

6. `v-leave-to`: 2.1.8版及以上 定义离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 v-leave 被删除)，在过渡/动画完成之后移除。

![alt transition](https://cn.vuejs.org/images/transition.png "过渡的类名")

对于这些在过渡中切换的类名来说，如果你使用一个没有名字的 `<transition>`，则 `v-` 是这些类名的默认前缀。如果你使用了 `<transition name="my-transition">`，那么 `v-enter` 会替换为 `my-transition-enter`。

`v-enter-active` 和 `v-leave-active` 可以控制进入/离开过渡的不同的缓和曲线。

## Animate.css

[animate.css](https://daneden.github.io/animate.css/)

<iframe width="100%" height="300" src="//jsfiddle.net/JacobHsu/gen98yao/7/embedded/result,js,html/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

```html
<head>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.min.css">
</head>
<div id="app">
  <button v-on:click="show = !show">
    Toggle
  </button>
  <transition name="fade"
    enter-active-class="animated swing"
    leave-active-class="animated shake">
    <p v-if="show">hello</p>
  </transition>
</div>
```

## 同时使用过渡和动画

<iframe width="100%" height="300" src="//jsfiddle.net/JacobHsu/bp7h0c3s/11/embedded/result,js,html,css/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

```html
<head>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.min.css">
</head>
<div id="app">
  <button v-on:click="show = !show">
    Toggle
  </button>
  <!-- type="transition" 告知以transition時長為準 3秒而不是1秒-->
  <!-- :duration="5000" 自定義動畫時長-->
  <!-- :duration="{enter: 5000, leave: 10000}" 自定義動畫時長-->
  <transition :duration="{enter: 5000, leave:10000}"
    name="fade"
    appear
    enter-active-class="animated swing fade-enter-active"
    leave-active-class="animated shake fade-leave-active"
    appear-active-class="animated swing">
    <p v-if="show">hello</p>
  </transition>
</div>
```

## Velocity.js

[Velocity.js](http://velocityjs.org/): An incredibly fast animation engine for motion designers.

Option [#beginAndComplete](http://velocityjs.org/#beginAndComplete)

<iframe width="100%" height="300" src="//jsfiddle.net/JacobHsu/e7kaj5sg/22/embedded/result,js,html,css/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>
