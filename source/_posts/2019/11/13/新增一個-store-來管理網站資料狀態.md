---
title: 新增一個 store 來管理網站資料狀態
abbrlink: 33419
date: 2019-11-13 01:02:27
tags:
  - Vue
  - Vuex
categories: Vue
thumbnail:
banner:
description:
---

這章節會將兩個各自透過變數 `Loading` 管理 `vue-loading-overlay` 的元件統一由 `store` 來管理

<!-- more -->

``` html
<template>
  <Loading :active.sync="isLoading"></Loading>
</template>

<script>
  export default {  
    data (){    
      return {      
        isLoading: false,    
      }  
    }
  }
</script>
```

## 安裝 Vuex

首先透過以下指令安裝 `Vuex`

``` 
npm install --save vuex
```

## 在進入點 main.js 啟用

下載後在進入點 `main.js` 啟用

``` js
// in main.js
            
import Vuex from 'vuex'
Vue.use(Vuex)
```

## 新增 store 管理檔案

可以透過新增一個 `store` 資料夾，並在其內新增一個 `index.js` 檔案來彙整所有 `vuex` 的行為，同樣的，需要先載入 `vue & vuex`

``` js
// in store/index.js
            
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```

載入 `vue & vuex` 後就可以透過 `ES6` 的 `export default` 來輸出資料內容

``` js 
export default new Vuex.Store({})
```

## 從進入點載入管理檔案

存檔後就可以在進入點 `main.js` 載入這支彙整 `vuex` 的檔案

``` js
// in main.js

import store from './store'

new Vue({
  store,
})
```

## 新增管理變數

`vuex` 會將原先由各自元件管理的變數統一由一個檔案來彙整，在這個範例中會將 `isLoading` 這個變數拉到 `state` 中管理

``` js
// in store/index.js

export default new Vuex.Store({
  state: {
    isLoading: false,
  }
})
```

## 載入管理變數

改由 `vuex` 管理變數後，原先在各元件中的變數就可以移除，並透過 `compoted` 來載入由 `vuex` 檔案來統一管理的變數 ; 而在這裡可以使用與原先相同的變數名稱 `isLoading` 來建立函式，就可以不用該改 `HTML` 元素中的設定

``` js
// in component

computed: {
  isLoading (){
    return this.$store.state.isLoading
  }
}
```

## 更改變數的值

過往透過更改變數的值來控制呈現狀態的方式，在這裡要更改的變數也要改成 `vuex` 檔案中的變數

``` js
// in component

methods: {
  getProducts (){
    const vm = this;
    const api = `test.api`;
    
    vm.$store.state.isLoading = true;
    vm.$http.get(api).then((response) => {
      vm.$store.state.isLoading = false;
    });
  }
}
```

## 刪除子元件元素

透過 `vuex` 統一管理變數的方式，子元件中的元素就可以刪除，並統一由父元件與 `vuex` 檔案來管理

``` html
<!-- in child component --> 

<template>
  <Loading :active.sync="isLoading"></Loading>  <!-- delete -->
</template>
```

## 資料參考

[官方資料](https://vuex.vuejs.org/zh/installation.html)

