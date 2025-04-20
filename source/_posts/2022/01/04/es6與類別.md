---
title: ES6與類別
abbrlink: 6272
date: 2022-01-04 23:46:12
tags:
categories: 克服JS的奇怪部分
thumbnail:
banner:
description:
---

<!-- @format -->

在過去的的課程中有提到，JavaScript 並不像其他程式語言一樣有 `class` 來定義並創造物件，但在 `ES6` 開始，JavaScript 有了 `class` 這個用法，但與其它程式語言的功能並不一樣，接下來讓我們來了解一下如何使用這個新用法吧。

<!-- more -->

## 建立一個 Class

讓我們直接透過程式碼來了解什麼是 `class`，首先 `class` 會有 `constructor` 來定義物件的屬性，且可以帶入參數來改變屬性的值，另外方法則可以直接寫在 `class` 內，而創立物件的用法就和 `function constructor` 一樣透過關鍵字 `new` 創立一個新的物件

```js
class Person {
  constructor(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
  }

  greet() {
    return "Hi" + firstname;
  }
}

var john = new Person("John", "Doe");
console.log(john);
// {
//   firstname: 'John',
//   lastname: 'Doe'
// }
```

這裡要注意的是，與其他程式語言不同，`class` 只是定義物件，它並不是一個物件，JavaScript 中的 `class` 不僅定義物件，它本身就是一個物件。

## Class Prototype

前面提到 JavaScript 中的 `class` 也是一個物件，因此它也可以繼承另一個 `class`，只要透過 `extend` 指向要繼承的目標 `class`，並透過 `super` 傳遞 `constructor` 的參數 ; 另外我也可以覆蓋掉原有的方法 `greet` 或屬性。

```js
class InformalPerson extend Person {

  constructor(firstname, lastname) {
    super(firstname, lastname);
  }

  greet() {
    return 'Yo ' + firstname;
  }

}
```

## Syntactic Sugar 語法糖

同樣的，如果專案需支援 `ES6` 以下的環境，我們就需需要撰寫 `polyfill` 來達到相同的效果，如同上一堂課提到的 `Object.create()` ; 在 JavaScript 中，`class` 被稱為一種語法糖 `Syntactic Sugar`，`Syntactic Sugar` 指的是在 JavaScript 底層其實是一模一樣的行為，只是我們將它包裝成另一種用法，如 `function constructor`、`Object.create()` 和 `class` 其實都是 prototypal inheritance。

## 資料來源

[Udemy-JavaScript: Understanding the Weird Parts](https://www.udemy.com/course/understand-javascript/)
