---
title: Closures and Callback
abbrlink: 6171
date: 2021-12-20 22:54:37
tags:
categories: 克服JS的奇怪部分
thumbnail:
banner:
description:
---

有了先前對於 Closures 的認知後，這堂課將進一步介紹 Closures 和回呼 (Callback) 

<!-- more -->

首先我看到一段程式碼如下:

建立一個 `sayHiLater`，並在其中設定一個 3 秒後印出 `greeting` 變數的計時器

```js
function sayHiLater() {

  var greeting = 'Hi!';

  setTimeout(function() {
    console.log(greeting);
  }, 3000);
}

sayHiLater();
```

結果會如預期一樣，在 3 秒後印出 `Hi!`，但進一步思考這段程式碼，為什麼 `setTimeout` 在 3 秒後還會知道 `greeting` 這個變數的值呢 ?

原因其實是這裡已經形成了一個 Closures，首先，在 `setTimeout` 函式中，我們有兩點可以特別留意 ; 第一，我們之所以可以把 `function` 當成參數傳入是因為在 JavaScript 中，`function` 屬於 `First Class Function` ，`function` 是一個 `object` ; 第二，我們直接在參數的位置透過 Function Expression 的方式建立一個 Anonymous Function

在函式 `sayHiLater` 執行完畢離開 Execution Stack 後，變數 `greeting` 仍然留在記憶體的該位置，所以當 3 秒後執行 `setTimeout` 時，會透過 `Scope Chain` 的概念取得 Outer Environment 的變數值，這裡便形成了一個 Closures。

## 常用的 `jQuery`

在 `jQuery` 中，一個常見的基本寫法如下:

```js
// jQuery uses function expressions and first-class functions !
$('button').click(function() {
  // do something
});
```

在 `click` 事件中放入 `function`，這就是運用了 first class function 把函式當成一般物件傳入作為參數，而把函式當作參數的也使用到了 callback function 的觀念，告訴這個函是當指定事件發生時，呼叫我給你的函式。

## Callback Function

`Callback Function` 指的是當我們呼叫一個 `function` 時，傳入另一個 `function` 當作參數，呼叫的 `function` 結束時，執行我們傳入的 `function`。

這裡我們看到一段範例程式碼如下:

傳入一個 `function` 作為參數，並在呼叫的 `function` 中執行我們傳入的 `function`

```js
function tellMeWhenDone(callback) {

  var a = 1000;   // some work
  var b = 2000;   // some work

  callback();
}

tellMeWhenDone(function() {
  console.log('I am done !');   // I am done !
});
```

## 資料來源

[Udemy-JavaScript: Understanding the Weird Parts](https://www.udemy.com/course/understand-javascript/)`
