---
title: payload 傳遞參數
abbrlink: 32048
date: 2019-11-13 23:53:07
tags:
  - Vue
  - Vuex
categories: Vue
thumbnail:
banner:
description:
---

當我們要從元件傳遞參數到 `actions` 時，如果原先在元件中的事件就有一個以上的參數，則第二個以後的參數就會出現 `undefined`

``` js
// in component
addtoCart(id, qty = 1) {
  this.$store.dispatch('addtoCart', id, qty);
}

// in store
actions: {
  addtoCart(context, id, qty) {
    console.log(context, id, qty);   // '預設 context 內容' 'id' undefined
  }
}
```

## 物件形式傳遞多個參數

如果原先事件就有一個以上的參數，就可以透過物件 `{}` 的形式將所有參數包起來傳遞，再透過 "解構" 的方式取出參數

``` js
// in component
addtoCart(id, qty = 1) {
  this.$store.dispatch('addtoCart', { id, qty });
}

// in store
actions: {
  addtoCart(context, { id, qty }) {
    console.log(context, id, qty);   // '預設 context 內容' 'id' 'qty'
  }
}
```


