---
title: Vuex 中的 Getters、mapGetters 與 mapActions
abbrlink: 26866
date: 2019-11-14 00:49:45
tags:
  - Vue
  - Vuex
categories: Vue
thumbnail:
banner:
description:
---

在先前有提到，`store` 中有一個類似原件中的 `computed` 的方法叫做 `getters`，當有多個原件要用到這個屬性時，可以將其放到 `store` 中供其他原件使用，同樣會在其依賴的值發生變化時重新計算

<!-- more -->

## getters 與 mapGetters

延續上一章節，我們將原先在原件 `computed` 中的屬性移到 `store` 中，`getters` 中的方法同樣在一個個參數可以帶入 `state`

``` js
// in component
computed: {
  categories() {
    return this.$store.state.categories;
  },
  products() {
    return this.$store.state.products;
  },
}
          
// in store
getters: {
  categories(state) {
    return state.categories;
  },
  products(state) {
    return state.products;
  },
}
```

將 `computed` 中的方法移到 `store` 中後，透過 `import` 解構的方式從 `Vuex` 中取得 `mapGetters` 這個方法，再透過展開 `...` 的方式在 `computed` 中載入 `getters` 屬性

``` js
// in component
import { mapGetters } from 'vuex';

export default {
  computed: {
    ...mapGetters(['categories', 'products']);
  }
}
```

## actions 與 mapActions

同樣的，`actions` 也可以透過 `mapActions` 這個方法載入，但這裡要注意的是，`mapActions` 只能應用在 "沒有參數" 的方法上，如果原件中的方法有地入參數，就只能透過 `methods` 來呼叫

``` js
// in compontent
import { mapGetters, mapActions } from 'vuex';
          
export default {
  methods: {
    addtoCart(id, qty = 1) {
      this.$store.dispatch('addtoCart', { id, qty });
    },
    ...mapActions(['getProducts']);
  },
  computed: {
    ...mapGetters(['categories', 'products']);
  }
}
```