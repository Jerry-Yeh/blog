---
title: JavaScript - ES6 - 函式預設值
abbrlink: 3401
date: 2019-10-22 14:35:03
tags:
  - ES6
  - JavaScript
categories: JavaScript
thumbnail:
banner:
description:
---

在 ES6 中，可以很簡單的給予函式參數預設值，只要在參數的位置加上等號 `=` 賦值即可，可以避免沒有帶入參數出現的錯誤

<!-- more -->

``` js
function add(x, y){
  console.log(x + y);
}

add();   // NaN

function add2(x = 1, y = 2){
  console.log(x+y);
};

add();   // 3
```

呼叫函式的參數都是 "由前往後"，當代入的參數數量未達到函式預設的參數數量，會優先帶入前面的參數

如果只帶入一個參數，而且只有預設第一個參數，還是會出現錯誤

``` js
function add(x = 1, y){
  console.log(x + y);
};

add(2);   // NaN
```

如果只帶入一參數，但有預設後面的參數，就會順利完成函式呼叫

``` js
function add(x, y = 3){
  console.log(x + y);
};

add(1);   // 4
```