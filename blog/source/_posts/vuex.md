---
title: Vuex
---

main.js
```js
import store from './store'
new Vue({
  store,
  render: h => h(App),
}).$mount('#app')
```

store/index.js
```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
    state: {
        count: 10,
        numb: 10086
    },
    getters: {
        add: (state, getter) => {
            state.count = getter.add;
            return state.count;
        },
    },
    mutations: {
        increment(state) {
            state.count = 2;
        },
    },
    actions: {
        actionA({ dispatch, commit }) {
            return commit('add');
        },
    }
}); 


export default store; 
```

