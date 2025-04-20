---
title: Framework 如何使用 Closures - Function Fatories
abbrlink: 39540
date: 2021-12-19 23:30:01
tags:
categories: 克服JS的奇怪部分
thumbnail:
banner:
description:
---

有了前幾堂課對於 Closures 的了解後，這堂課將介紹 Framework 如何使用 Closures

<!-- more -->

以下我們先來看一段程式碼

首先建立一個 `makeGreeting` 函式，帶入一個 `language` 變數，並透過判斷這個變數來回傳對應的函式內容，接著透過兩個變數帶入不同的參數來呼叫這函式，最後呼叫這兩個指派給變數的函式

```js
function makeGreeting(language) {

  return function(firstname, lastname) {

    if (language === 'en') {
      console.log('Hello' + firstname + lastname);
    }

    if (language === 'es') {
      console.log('Hola' + firstname + lastname);
    }
  }
}

var grettingEnglish = makeGreeting('en');
var grettingSpanish = makeGreeting('es');

grettingEnglish('John', 'Doe');   // Hello John Doe
grettingSpanish('John', 'Doe');   // Hola John Doe
```

結果就是得到各自預期的字串

## 解析程式碼

以下我們依序解析這段程式碼

首先，在 JavaScript Engine 開始時，因為 Hoisting 的緣故，`makeGreeting()`、`greetEnglish` 和 `greetSpanish` 會被儲存在記憶體中，接著才會開始執行程式碼

![](Hoisting.png)

在執行到 `grettingEnglish` 時，`makeGreeting` 的 Execution Context 會被建立，同時將 `language = en` 儲存在記憶體中，最後離開 Execution Stack，同理 `greetSpanish` 也會儲存 `language = es` 在記憶體的另一個位置

![](greetingEnglish.png)

接著當執行到 `greetEnglish('John', 'Doe')` 時，會產生一個新的 Execution Context，當執行到 `lanuage` 這個變數時，因為在函式內找不到，便會透過 `Scoped Chain` 找到外部的 `en`，這整個環境便形成了一個 Closures

![](Closures.png)

程式碼往下執行到最後一行 `greetSpanish` 時，會按照相同的流程形成第二個 `Closures`。

![](Second-Closures.png)

這段程式碼指出的 JavaScript 特色是，每執行一次函式，都會產生一個 Execution Context，JavaScript Engine 會指向該函式對應的變數，透過函式來創造函式，可以讓我們容易使用並保留預設參數，`makeGreeting` 這個函式我們就稱它為 Function Factories。

## 資料來源

[Udemy-JavaScript: Understanding the Weird Parts](https://www.udemy.com/course/understand-javascript/)`
