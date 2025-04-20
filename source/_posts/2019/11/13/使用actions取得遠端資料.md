---
title: 使用 actions 取得遠端資料
abbrlink: 7550
date: 2019-11-13 23:46:46
tags: 
  - Vue
  - Vuex
categories: Vue
thumbnail:
banner:
description:
---

`Vuex` 最大的特色就是可以將 "重複" 使用到的變數與方法統一放到 `store` 來管理，而其管理的流程是透過元件來呼叫 `actions` 來觸發 "非同步" 的事件，再透過 `commit` 呼叫 `mutations` 中的同步事件來改變 `state` 中的資料狀態，進而影響畫面的呈現

<!-- more -->

在這個章節中，我們將原先各自放在元件中的事件拉到 `store` 中統一管理，以下是原先在元件中的資料

``` js
// in component

export default {
  name: 'Home',
  data() {
    return {
      products: [],
      categories: [],
    }
  },
  methods: {
    getProducts() {
      const vm = this;
      const url = `${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/products/all`;
      vm.$store.dispatch('updateLoading', true);
      this.$http.get(url).then((response) => {
        vm.products = response.data.products;
        console.log('取得產品列表:', response);
        vm.getUnique();
        vm.$store.dispatch('updateLoading', false);
      });
    },
    getUnique() {
      const vm = this;
      const categories = new Set();
      vm.products.forEach((item) => {
        categories.add(item.category);
      });
      vm.categories = Array.from(categories);
    },
  },
  created() {
    this.getProducts();
  },
}
```

首先將 `getProducts` 事件整個拉到 `actions` 中，並帶入第一個參數 `context`

``` js
actions: {
  getProducts(context) {
    const vm = this;
    const url = `${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/products/all`;
    vm.$store.dispatch('updateLoading', true);
    this.$http.get(url).then((response) => {
      vm.products = response.data.products;
      console.log('取得產品列表:', response);
      vm.getUnique();
      vm.$store.dispatch('updateLoading', false);
    });
  }
}
```

接著修改以下項目 :

## this

原先在元件中的 `this` 指的是元件本身，在這裡並不指向元件，所以先將其刪除

## this.$store.dispatch 方法

原先透過 `dispatch` 這個方法來觸發 `actions` 中方法的方式，可以改成直接呼叫

``` js
// 原先方法
this.$store.dispatch('updateLoading', true);   
vm.getUnique();
vm.products = response.data.products;
this.$store.dispatch('updateLoading', false);   

// 直接呼叫
context.commit('LOADING', true);
context.commit('CATEGORITES', response.data.products);
context.commit('PRODUCTS', response.data.products);
context.commit('LOADING', false);
```

## 儲存資料

而原先儲存資料的行為則放到 `mutations` 中管理，在這裡透過 "常數" 命名，所以名稱上不會有衝突

``` js
matations: {
  PRODUCTS(state, payload) {
    state.products = payload;
  },
  CATEGORITES(state, payload) {
    const categories = new Set();
    payload.forEach((item) => {
      categories.add(item.categories);
    });
  }
}
```

## 從元件呼叫 store 事件

原先在元件中的事件因為其中的非同步語同步都各自搬到 `actions` 與 `mutations` 中統一管理，所以在這裡只要呼叫 `store` 中的事件即可

``` js
methods: {
  getProducts() {
    this.$store.dispatch('getProducts');
  },
}
```

## axios 取得遠端資料

在原先的元件中，我們透過 `axios` 這個套件中的方法來取得遠端資料，在 `Vue` 元件中，透過 `vue-axios` 的方法 `this.$http` 來操做，但是在 `store` 中只能透過 `axios` 原始的方法來操作，所以在這裡要先載入 `axios` 這個套件

``` js
import axios from 'axios';

this.$http.get(api).then();   // 元件中方法
axios.get(api).then();   // 直接使用 axios 方法
```

## 將資料呈現畫面上

最後再透過 `computed` 監控資料，在資料改變時同步改變畫面呈現，並刪除原先在元件中的變數

``` js
// in component

export default {
  data() {
    return {
      products: [],   // delete
      categories: [],   // delete
    }
  },
  computed: {
    categories() {
      return this.$store.state.categories;
    },
    products() {
      return this.$store.state.products;
    }
  }
}
```


