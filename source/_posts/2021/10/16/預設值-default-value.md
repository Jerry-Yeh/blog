---
title: 預設值 Default Value
categories: 克服JS的奇怪部分
abbrlink: 58810
date: 2021-10-16 16:17:21
tags:
thumbnail:
banner:
description:
---

這章節會透過其他角度來看 JavaScript 強制轉型的特性

<!-- more -->

首先先宣告一個擁有一個參數的函式，我們都知道 JavaScript 的變數如果沒有主動賦值，都會被給予一個 `undefined`，所以程式並不會出錯

```js
function greet(name) {
  console.log('Hello ' + name);
}

greet();        // Hello undefined
greet(Jerry);   // Hello Jerry  
```

接著將變數改成如果沒有賦值，我們透過 `||` 給予一個預設值

```js
function greet(name) {
  name = name || '<Your name here>';
  console.log('Hello ' + name);
}

greet();   // Hello <Your name here>
```

原因是 `||` 優先於 `=`，而 `||` 會回傳優先為 `true` 的值，所以如果沒有賦與參數，會回傳透過 `Boolean` 轉型為 `true` 的值

```js
function greet(name) {
  name = false || true;
  console.log('Hello ' + name);
}

greet();   // // Hello <Your name here>
```