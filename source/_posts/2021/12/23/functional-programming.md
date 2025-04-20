---
title: Functional Programming
abbrlink: 51899
date: 2021-12-23 21:41:03
tags:
categories: 克服JS的奇怪部分
thumbnail:
banner:
description:
---

有了之前對於 First Class Function 的了解後，這堂課將介紹 JavaScript 程式設計很強大的地方 - Functional Programming

<!-- more -->

在 JavaScript 有一個很特別的地方在於函式屬於物件，所以可以透過匿名函式 (Anonymous Function) 的方式指派一個變數，並透過 `return` 回傳一個值，這種方法就稱為 First Class Function，這是在其他程式語言中不太會見到的。

首先看到一段程式碼如下:

建立一個陣列，接著建立第二個空陣列，並透過 `for` 迴圈 `push` 2 倍的值進入第二個空陣列，如此一來就可以得到和第一個陣列長度相同且兩倍的值。 

```js
var arr1 = [1, 2. 3];

var arr2 = [];
for (var i = 0; i < arr1.length; i++) {

  arr2.push(arr1[i] * 2);

}

console.log(arr2);   // [2, 4, 6]
```

但身為一個程式開發人員，我們總是想要透過最少的程式碼來達到相同的目的，且避免重複撰寫類似的程式碼，所以我們試著透過 Functional Programming 的概念來改寫這段程式碼。

首先，建立一個函式，並將這個 `for` 迴圈放入函式內，接著將第一個陣列作為第一個參數傳入，再將要對陣列做的 Callback Function 做為第二個參數傳入。

```js
function mapForEach(arr, fn) {

  var newArr = [];
  for(var i = 1; i < arr.length; i++) {
    newArr.push(
      fn(arr[i]);
    );
  }

  return newArr;
}
```

為了達到和原先一樣把陣列的所有值乘上 2 的目的，只要透過呼叫 `mapForEach` 這個函式，並將要處理的陣列帶入第一個參數，接著把要處理的條件透過 `function expressions` 帶入第二個參數:

```js
var arr1 = [1, 2, 3];
var arr2 = mapForEach(arr1, function(item) {
  return item * 2;
});

console.log(arr2);   // [2, 4, 6]
```

## Functional Programming 不同應用

透過 `Functional Programming` 的方式，我們可以輕易地對 `array` 做到不同的處理 ; 如上，第一個範例是將所有值乘上 `2`，如果要判斷 `array` 中的值是否大於 `2`，也可以透過呼叫 `mapForEach` 做到:

```js
var arr3 = mapForEach(arr1, function(item) {
  return item > 2;
});

console.log(arr3);   // [false, false, true]
```

如此一來就可以透過 `mapForEach` 中的第 2 個 `function` 參數對 `array` 做到不一樣的效果。

## Function Programming 進階用法

延續上方的範例，我們來看到一個更進階的用法，如果連判斷是否大於 `2` 的條件都可以彈性透過參數帶入我們可以怎麼做呢 ?

首先建立一個函式來判斷傳入的參數 `item` 是否大於條件 `limiter`。

```js
function mapForEach(arr, fn) {

  var newArr = [];
  for (var i = 0; i < arr.length; i++) {

    newArr.push(
      fn(arr[i])
    );
  };

  return newArr;

}

var checkPastLimit = function(limiter, item) {
  return item > limiter;
}
```

寫到這裡，會有一個疑問是，`mapForEach` 在呼叫這個函式的時候，只有帶入一個參數 `fn(arr[i])`，那麼要如何給定 `limiter` 一個預設值呢 ?

在這裡就可以用到前面章節所學到的 `bind()` 來帶入預設的參數了，只要在使用 `bind()` 的時候，預先帶入一個參數，這個參數就會是 `limiter` 的值，接著後續呼叫所帶入的值自然就會是 `item` 了。

```js
var arr1 = [1, 2, 3];
var arr4 = mapForEach(arr1, checkPastLimit.bind(this, 1));

console.log(arr4);
// [false, true, true]
// (1 > 1, 2 > 1, 3 > 1)
```

## 簡化 checkPastLimit 到只帶入一個參數

接著，我們進一步來簡化上述範例，讓這個函式在使用時，不用每次都透過 `bind()` 來帶入 `this` 與參數，而是只要透過帶入一個參數作為 `limiter` 即可。

在這裡，我們要做的其實就只是將 `bind()` 的應用包裝成另一個函式，並給定一個參數 `limiter` 帶入:

```js
var checkPastLimitSimplified = function(limiter) {
  return function(limiter, item) {
    return item > limiter;
  }.bind(this, limiter);
};
 
var arr5 = mapForEach(arr1, checkPastLimitSimplified(1));
console.log(arr5);   // [false, true, true]
```


## 資料來源

[Udemy-JavaScript: Understanding the Weird Parts](https://www.udemy.com/course/understand-javascript/)`
