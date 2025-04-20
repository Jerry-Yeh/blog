---
title: 存在與布林 Existence and Booleans
abbrlink: 61081
date: 2021-10-16 09:32:31
tags:
categories: 克服JS的奇怪部分
thumbnail:
banner:
description:
---

在 JavaScript 中，我們可以透過 `Boolean` 來判斷一個值是否存在，某些值被轉型為布林後會被判斷為 `false`，如下:

<!-- more -->

```js
Boolean(undefined);   // false
Boolean(null);        // false
Boolean('');          // false
Boolean(0);           // false
```

表示這些值都不存在，但其實一般開發並不太會使用 `Boolean` 這個函式直接強制轉型，而是會透過 `if-else` 來判斷
如下範例，如果沒有為一個參數主動賦值，預設值就是 `undefined`，因此會被強制轉型為 `false`

```js
var a;

// goes to internet and looks for a value

if (a) {
  console.log('Something is there.');
}
```

接著看到如果賦值為 `0`，也會被強制轉型為 `false` ; 但如果再加上一個 `||` 使值為 `0` 的時候也通過，判定的結果就會正確，因為 `===` 優先於 `||`，在 `false` 或 `true` 的結果，結果就會是 `true`

```js
var a = 0;

// 初始條件
if (a || a === 0) {}

// === 運算子優先運算
if (a || true) {}

// || 運算子由左到右，0 被轉型為 false
if (false || true) {}
```





