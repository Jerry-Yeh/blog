---
title: 陣列與 for in
abbrlink: 39808
date: 2022-01-03 23:29:50
tags:
categories: 克服JS的奇怪部分
thumbnail:
banner:
description:
---

這堂課將介紹一個很容易誤會的用法，`Array` & `for in`

<!-- more -->

還記得在 {% post_link reflection-and-extend reflection and extend %} 這堂課中我們透過 `for` 迴圈將物件的屬性與方法擴充到另一個物件，而在 JavaScript 中，陣列也是物件，所以我們可以透過 `for in` 的方式取得陣列中的索引與值 `name value pair`

```js
var arr = ['John', 'Jane', 'Jim'];

for(var prop in arr) {
  console.log(prop + ': ' + arr[prop]);
}

// 0 John
// 1 Jane
// 2 Jim
```

在單純透過中括號建立物件 `literal array` 時，其實也是 `new` 一個物件，所以它的 `prototype` 也會指向 `Array.prototype`，所以如果我們在這上方直接新增屬性或方法，新建立的陣列也會取得，但透過 `for in` 列出陣列的 `name value pair` 時，新增在 `Array.prototype` 的屬性與方法也會被列出

```js
Array.prototype.myCustomFeature = 'cool!';

var arr = ['John', 'Jane', 'Jim'];

for(var prop in arr) {
  console.log(prop + ': ' + arr[prop]);
}

// 0: John
// 1: Jane
// 2: Jim
// myCustomFeature: cool!
```

所以在開發 `Array` 相關程式時，避免使用 `for in` 方式遍歷它的內容，使用標準的 `for` 迴圈即可，就不會出現 `prototype` 所新增的屬性與方法了。

```js
for(var i = 0; i < arr.length; i++>) {
  // do something ...
}
```

## 資料來源

[Udemy-JavaScript: Understanding the Weird Parts](https://www.udemy.com/course/understand-javascript/)`