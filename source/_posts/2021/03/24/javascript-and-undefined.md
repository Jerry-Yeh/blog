---
title: JavaScript and undefined
abbrlink: 27183
date: 2021-03-24 00:07:29
tags:
categories: 克服JS的奇怪部分
thumbnail:
banner:
description:
---

這章節會進一步套討變數的的預設值 undefined 與 not undefined 的差異

<!-- more -->

一段基本的變數宣告如下

```js
var a;

console.log('a', a);
console.log('b', b);

// a undefined
// b not defined
```
從以上結果來套討 undefined 與 not defined 的差異

### undefined

在 JavaScript 中值為 `undefined` 的情況並非沒有值，而是在創造階段已被宣告且在記憶體中佔有空間，只是還未被主動賦予一個值，所以 JavaScript 預設都會賦予 `undefined` 這個特殊關鍵字

### not defined

而 not defined 就不同了，由上面的結果可以知道 b 完全沒有宣告，所以在執行階段找不到這個值所以出現 `not defined` 的提示

### 主動賦予 undefined

```js
var a = undefined;

console.log(a === undefined);   // true
```

由上面的結果可以得知如果主動賦予變數 undefined 在 JavaScript 中是成立的，但這麼做是危險且不建議的，原因是 `undefined` 這個值為 JavaScript 在變數已宣告但未賦值時的預設值，所以如果這麼做就會難以區分這個變數為 `undefined` 的原因