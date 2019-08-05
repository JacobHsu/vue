---
title: Input
---

# Input

vue 设置文本框只能输入正整数

```js
<el-input @keyup.native="handleInput" v-model="form.subjectId" :placeholder=""></el-input>
```

```js
// 验证只能输入正整数
handleInput() {
    this.form.subjectId = this.form.subjectId.replace(/[^\.\d]/g, '')
    this.form.subjectId = this.form.subjectId.replace('.', '')
}
```

[Vue键盘事件为何要加上native？](https://segmentfault.com/q/1010000009478912)  
现在在组件上使用 v-on 只会监听自定义事件 (组件用 $emit 触发的事件)。
如果要监听根元素的原生事件，可以使用 `.native` 修饰符