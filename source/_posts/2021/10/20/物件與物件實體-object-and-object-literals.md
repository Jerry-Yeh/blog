---
title: 物件與物件實體 Object and Object Literals
tags:
  - 物件與函式
categories: 克服JS的奇怪部分
abbrlink: 30806
date: 2021-10-20 13:06:08
thumbnail:
banner:
description:
---

延續上一章節最後提到使用 `new` 來建立一個物件並不是很好的方法，這章節會針對這點做說明，並介紹何為物件實體 (Object Literals)

<!-- more -->

## 使用大括號 {} 建立物件

上一章節透過 `new` 建立物件，並透過點. (Dot) 或中括號 [] (Bracket) 來擴增物件屬性

```js
var person = new Object();

person.firstname = 'Tony';
```

但其實在建立物件時，有更快的作法，我們可以透過大括號 {} 直接建立一個物件，這裡要注意的是，大括號 {} 並不是一個運算子，在 JavaScirpt 中使用大括號 {} 就會預設你建立了一個物件

```js
var person = {};
```

當然，我們可以做到更多，包含物件屬性 (Property) 和方法 (Method) 的建立，這樣的建立方式比起透過 `new` 來建立物件再一個一個增加屬性快上許多

```js
var person = {
  firstname: 'Tony',
  lastname: 'Alicea',
  address: {
    street: '111 Main St.',
    city: 'New York',
    state: 'NY'
  }
};

person = new Object();
person.firstname = 'Tony';
person.lastname = 'Alicea';
person.address.street = '111 Main St.';
person.address.city = 'New York';
person.address.state = 'NY';
```

## 搭配函式使用物件

延續上方當我們建立好函式後，可以搭配函式來使用建立好的物件

```js
var Tony = {
  firstname: 'Tony',
  lastname: 'Alicea',
  address: {
    street: '111 Main St.',
    city: 'New York',
    state: 'NY'
  }
};

function greet(person) {
  console.log('Hi' + person.firstname);
}

greet(Tony);   // Hi Tony
```

我們也可以透過呼叫函式時建立物件

```js
greet({
  firstname: 'Mary',
  lastname: 'Doe'}
);   // Hi Mary
```

## 補充

無論是使用 `new` 關鍵字或大括號 {} 來建立物件，對於 JavaScript 底層來說都是一樣的，本質上都是建立物件到記憶體中，並建立該物件的屬性 (Property) 和方法 (Method) 於記憶體中。



