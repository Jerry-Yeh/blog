---
title: arguments 與 spread
abbrlink: 63221
date: 2021-12-15 23:43:35
tags:
categories: 克服JS的奇怪部分
thumbnail:
banner:
description:
---

這堂課將會介紹在 JavaScript 中，當 Function 被建立時，會自動產生的關鍵字 Arguemnts

<!-- more -->

當我們執行一個 Function 時，執行環境 (Execution Context) 會被建立，然後產生一個變數環境 (Variable Environment) 來幫我們保存變數和一個外部外部環境 (Outer Environment) 來判斷 Scoped Chain; 接著會產生 `this` ，依據函式的呼叫方式而指向不同的 `object`; 最後一個關鍵字則是這堂課的主題 `arguments`

![](Execution-Context-is-Created.png)

在 JavaScript 中，當我們建立一個函式並呼叫它時，即使不給予預期的參數，也不會出錯，只會在用到參數時回傳 `undefined`

```js
function greet(firstname, lastname, language) {
  console.log(firstname);
  console.log(lastname);
  console.log(language);
}

greet();   // undeined undeined undeined
```

當然我們也可以透過修改條件判斷是否有參數傳入而給予參數一個預設值

```js
function greet(firstname, lastname, language) {
  language = language || 'en';
  console.log(fistname);
  console.log(lastname);
  console.log(language);
} 

console.log('John');   // John undefined en
```

除了主動設定的參數外，JavaScript 會自動幫我們建立一個名為 `arguments` 的 array-like 關鍵字來包含所有傳入的參數，它看起來像是一個 `Array`，但又不包含 `Array` 的所有特性;如下範例，我們可以和 `Array` 一樣透過 `length` 來判斷是否為空，也可以透過 [] (brakets operator) 來取得 `arguments` 中的值

```js
function greet(firstname, lastname, language) {
  if (arguments.length === 0) {
    console.log('Missing parameters !');
  }
  console.log(fistname);
  console.log(lastname);
  console.log(language);
  console.log(arguments);
  console.log(arguments[0]);
  console.log('-----');
} 

greet();
// Missing parameters !
// undefined
// undefined
// undefined
// []
// undefined
// -----
greet('John', 'Doe', 'en');
// John
// Doe
// en
// ['John', 'Doe', 'en']
// John
// -----
```

## 展開運算子 spread (...)

在 ES6 中有一個提供了一個新的展開運算子 spread (...)，它可以將後續傳入的所以參數集結成一個新的變數

```js
function greet(firstname, lastname, language, ...other) {
  console.log(firstname);
  console.log(lastname);
  console.log(language);
  console.log(other);
  console.log(arguments);
}
greet('John', 'Doe', 'en', 'St.', 'Taiwan');
// John
// Doe
// en
// ['St.', 'Taiwan']
// ['John', 'Doe', 'en', 'St.', 'Taiwan']
```

## 資料來源

[Udemy-JavaScript: Understanding the Weird Parts](https://www.udemy.com/course/understand-javascript/)
