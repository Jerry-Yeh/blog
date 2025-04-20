---
title: typeof and instanceof
abbrlink: 35648
date: 2022-01-05 23:51:37
tags:
categories: 克服JS的奇怪部分
thumbnail:
banner:
description:
---

這堂課將介紹兩個實用的內建語法，`typeof` and `instanceof`

<!-- more -->

## typeof

當我們都過 `typeof` 加上一個空格後，放上我們要辨識的變數，它就會回傳一個該變數型別的字串，如下:

```js
var a = 3;
console.log(typeof a);   // number

var b = 'Hello';
console.log(typeof b):   // string

var c = {};
console.log(typeof c);   // object
```

而因為陣列也是物件，所以透過 `typeof` 查看陣列型別時，也會回傳字串 `object`，但這裡有一個更實用的方法來辨別一般物件與陣列，只要將 `Object.prototype` 的方法 `toString` 透過 `call` 將 `this` 指向陣列，它就不會單純回傳一個字串 `object`

```js
var d = [];
console.log(d);   // object
console.log(Object.prototype.toString.call(d));   // [object Array]
```

但如果直接在陣列呼叫 `toString`，它會將陣列中的值字串化，如陣列 `d` 是一個空陣列，因此回傳空值

```js
console.log(d.toString());   //
```

而 `undefined` 的型別就是 `undefined`

```js
console.log(typeof undefined);   // undefined
```

### Bug

但這裡有一個 `JavaScript` 的 Bug，而且它也沒有打算修復，因為這個 Bug 已經存在很久了，如果修復它會影響很多已存在的專案和 library ; 那就是透過 `typeof` 查看 `null` 時，會回傳字串 `object`

```js
console.log(typeof null);   // object
```

## instanceof

`instanceof` 則是會判斷前面的值是否可以在它的 `prototypal chain` 找到後面的值，如下範例:

先建立一個 function constructor - `Person`，再透過 `new` 建立新的物件 `e` ，因為 `e` 的 `prototype` 就是 `Person`，所以回傳 `true`。

```js
function Person(name) {
  this.name = name;
}

var e = new Person('Jane');
console.log(typeof e);   // object
console.log(e instanceof Person);   // true
```


## 資料來源

[Udemy-JavaScript: Understanding the Weird Parts](https://www.udemy.com/course/understand-javascript/)
