---
title: Scope、ES6 & let
abbrlink: 41213
date: 2021-04-03 15:48:45
tags:
categories: 克服JS的奇怪部分
thumbnail:
banner:
description:
---

前面章節所提到變數環境、詞彙環境定義了 '範圍'，'範圍'是變數可以被取用的區域

<!-- more -->

下一版本的 JavaScript 稱作 ECMAScript 6、ECMAScript 2015 或 ES6，這個版本提供了新的變數宣告方式 `let`

### let

`let` 並非完全取代 `var`，`var` 還是存在，但 `let` 讓 JavaScript 引擎使用一種叫做區塊範圍 (block scoping) 的東西，所謂區塊的定義包括 `if` & `for` 的 大括號 {}，所以如果在執行 `for loop` 時，每次變數在記憶體都是指向不同的位置

```js
if (a > b) {
    let c = true;
} 
for () {
    let d = true;
}
```
