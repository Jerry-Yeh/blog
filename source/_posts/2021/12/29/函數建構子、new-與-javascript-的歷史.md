---
title: 函數建構子、new 與 JavaScript 的歷史
abbrlink: 60572
date: 2021-12-29 23:26:04
tags:
categories: 克服JS的奇怪部分
thumbnail:
banner:
description:
---

在過去的章節介紹過物件最基本的建立方式 - literal object，這堂課將透過 JavaScript 的歷史來介紹物件的其他建立方式。

<!-- more -->

JavaScript 在創立初期，作者為了吸引 Java 的開發者使用它，所以在命名上便參考 Java 這個字，但在開發上其實有非常大的不同 ; 其中 `new` 這個關鍵字便是模仿 Java 而來，但在 Java 中，`new` 是用來建立一個 `class`，`class` 並非 `object`，而是用來定義並建立 `object` ; 然而在 JavaScript 中，`new` 則是被用來建立 `object`，這也是本堂課將介紹的用法 ; JavaScript 在 ES6 後的本版也有 `class` 這個關鍵字，但用法則和 `Java` 完全不同。

## new object

前面提到，JavaScript 中的關鍵字 `new` 和 `Java` 中的用法並不一樣，那麼當我們透過 `new` 來建立物件的時候到底會發生什麼事呢 ?

讓我們透過一段程式碼範例來了解:

首先，建立一個函式 `person`，接著透過 `new` 來呼叫這個函式並指派給一個變數 ` john`

```js
function Person() {

  console.log(this);
  this.firstname = 'John';
  this.lastname = 'Doe';
  console.log('This function is invoked.');

}

var john = new Person();
console.log(john);
```

讓我們依序拆解這段程式碼，首先，呼叫函式 `Person()` 其實就和一般呼叫函式相同，會直接執行函式內的程式碼，如果沒有特別 `return`，就會回傳 `undefined`，接著前面的關鍵字 `new` 會建立一個空物件 `{}`，並將函式的 `this` 指向這個物件，所以當我們透過 `console.log` 查看 `john` 時，會得到一個含有兩個屬性 `firstname` 和 `lastname` 的物件。

![](new-object.png)

那麼如果再透過 `new` 函式來建立第二個物件呢 ?

```js
var jane = new Person();
console.log(jane);
```

會和第一個物件 `john` 得到一個含有兩個屬性的物件。

![](new-two-object.png)

但如果我們希望透過 `new` 來建立的物件能自由地賦予它屬性的值該怎麼做呢 ?

前面提到，這種建立物件的方式其實會先呼叫這個函式，再將它的 `this` 指向一個新產生的空物件，所以就如同一般函式，我們可以傳入參數，並指派給它的屬性，這麼一來就可以達到自由賦予屬性值的目的了。

```js
function Person(firstname, lastnmae) {

  this.firstname = firstname;
  this.lastname = lastname;

}

var john = new Person('John', 'Doe');
console.log(john);   // {firstname: 'John', lastname: 'Doe'}

var jane = new Person('Jane', 'Doe');
console.log(jane);   // {firstname: 'Jane', lastname: 'Doe'}

```

這種被用來透過關鍵字 `new` 來建立物件並回傳將 `this` 指派給新生成空物件的函式就稱為 Function Constructors。

## 資料來源

[Udemy-JavaScript: Understanding the Weird Parts](https://www.udemy.com/course/understand-javascript/)`
