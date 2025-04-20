---
title: JavaScript - ES6 - 變數
tags: 
  - JavaScript
  - ES6
categories: JavaScript
abbrlink: 50209
date: 2019-10-13 23:18:19
thumbnail:
banner:
description:
---

在 ES6 中多了兩種變數的宣告方式，`let` 與 `const`，其最大的目的在於盡量避免汙染到全域，原因在於載入多個模組或進行多人協作時，如果遇到相同名稱的全域變數就會發生衝突，也減少了程式的靈活性

<!-- more -->

## window 與 var

在 ES6 以前，都使用 `var` 來進行變數的宣告，而切分 `var` 有效範圍的最小單位是 函式 function，所以在 `function` 外使用 `var` 宣告的變數都會成為全域變數，也就是 最高級物件 `window` 的屬性

``` js
var a = 0;
console.log(a);   // 0

for (var b =0; b < 2; b++){
  console.log(b);   // 0, 1
};
console.log(b);   // 2

(function (){
  var c = 3
  console.log(c);   // 3
})();
console.log(c);   // c is not defined

console.log(window.a);   // 0
console.log(window.b);   // 3
console.log(window.c);   // undefined
```

## let 與 var

與 `var` 不同的是，`let` 是一個 區塊 `block` 變數，`block` 意指 大括號 `{ }`， 也就是說切分 let 有效範圍的最小單位是 大括號 `{ }`，且使用 let 宣告的全域變數不會成為全域物件 `window` 的屬性

同上範例，我們將 `var` 都改成 `let`

``` js
let a = 0;
console.log(a);   // 0

for (let b =0; b < 2; b++){
  console.log(b);   // 0, 1
};
console.log(b);   // b is not defined

(function (){
  let c = 3
  console.log(c);   // 3
})();
console.log(c);   // c is not defined

console.log(window.a);   // undefined
console.log(window.b);   // undefined
console.log(window.c);   // undefined
```

## let 與 for 的用法

如下範例，我們要讓每一個 `li` 被點擊時都能顯示自己的數字

``` html
<ul class="list">
  <li>1</li>
  <li>2</li>
  <li>3</li>
</ul>
```

``` js
for (var i = 0; i < 2; i++){
  document.querySelectorAll('.list li')[i].addEventListener('click',function (){
    console.log(i + 1);
  })
}
```

但結果並不如預期，在點擊每個 `li` 都會是 `4`，因為這裡的變數 `i` 使用 `var` 來宣告，所以是全域變數，當跑完 for 迴圈的時候，所有 `console.log(i + 1)` 中的 `i` 都會被全域的 `i = 3` 取代

如果將上述範例的 `var` 改成 `let`

``` js
for (let i = 0; i < 2; i++){
  document.querySelectorAll('.list li')[i].addEventListener('click',function (){
    console.log(i + 1);
  })
}
```

改成 `let` 來宣告的結果會如預期一樣，點擊每個 `li` 都是自己的數字，因為變數 `i` 並不會成為全域變數，所以 `console.log(i + 1)` 中的 `i` 都會是迴圈執行時當下取得的值

``` js
for (let i = 0; i < 2; i++){
  document.querySelectorAll('.list li')[i].addEventListener('click',function (){
    console.log(i + 1);
  })
}
```

改成 let 來宣告的結果會如預期一樣，點擊每個 li 都是自己的數字，因為變數 i 並不會成為全域變數，所以 console.log(i + 1) 中的 i 都會是迴圈執行時當下取得的值

## const

* 使用 `const` 宣告的變數又稱為 "常數" 或 "唯讀變數"
* 宣告完的變數無法再進行修改

  ``` js
  const a = 1;
  a = 2;  
  console.log(a);   // 錯誤
  ```

* 但使用 `const` 宣告的 物件 `{ }` 和 陣列 `[]` 依然可以修改其內容，因為物件型別是 "傳址" `pass by reference`

  ``` js
  const obj = {
    url: 'a'
  };
  obj.url = 'b';
  console.log(obj.url);   // b
  ```

* 如果要使宣告的物件連屬性也無法修改，可以使用 `Object.freeze()` 來將物件凍結

  ``` js
  const obj = {
    url: 'a'
  };
  Object.freeze(obj);
  obj.url = b;
  console.log(obj.url);   // a
  ```

## let 與 const 的注意事項

ES6 變數的宣告與以往使用 `var` 有一些性質上的不同，以下一一介紹

* 不會向上提升 Hoisting，從宣告的那一行開始成立

  ``` js
  var d;
  console.log(a);   // undefined
  console.log(b);   // ReferenceError
  console.log(c);   // ReferenceError
  console.log(d);   // undefined
  var a = 1;
  let b = 2;
  const c = 3;
  ```

* 同一個 "區塊" `block` 內不能重複命名

  ``` js
  var a = 1;
  var a = 2;
  console.log(a);   // 2

  let b = 1;
  let b = 2;   // SyntaxError
  const c = 1;
  const c = 2;   // SyntaxError
  ```

* 在全域宣告的變數不會成為最高級物件 `window` 的屬性

  ``` js
  var a = 1;
  let b = 2;
  const c = 3;
  console.log(window.a);   // 1
  console.log(window.b);   // undefined 
  console.log(window.c);   // undefined 
  ```

## 資料參考

* [JavaScript 入門篇 - 學徒的試煉](https://www.udemy.com/javascript-learning/)
* [邁向 JavaScript 勇者之路](https://ithelp.ithome.com.tw/users/20083608/ironman/1354)
* [ECMAScript 6 入門](http://es6.ruanyifeng.com/#docs/intro)
