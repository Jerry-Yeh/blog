---
title: Reflection and Extend
abbrlink: 40176
date: 2021-12-28 23:09:52
tags:
categories: 克服JS的奇怪部分
thumbnail:
banner:
description:
---

<!-- @format -->

這堂課將來介紹一個在很多 Libaries 會使用到的觀念 - Extend，而 Extend 又因為 Reflection 而成立，讓我們來了解一下這兩個名詞吧。

<!-- more -->

首先，來了解一下 Reflection 這個名詞，Reflection 指的是物件可以取得並更改自己的屬性 `properties` 和方法 `method`。

延續上次堂課的範例，我們建立兩個物件 `person` 和 `john`，並將 `john` 的 `prototype` 指向 `person`

```js
var person = {
  firstname: "Default",
  lastname: "Default",
  getFullName: function () {
    return firstname + " " + lastname;
  },
};

var john = {
  firstname: "John",
  lastname: "Doe",
};

// don't do this EVER! for demo purposes only !!
john.__proto__ = person;
```

## for in 基本應用

接著透過 `for...in` 列出物件 `person` 中的所有 `properties` 與 `method`，也就是每個 `for` 迴圈的變數 `prop`，結果會發現連 `prototype` 的方法 `getFullName` 也被囊括進去了。

```js
for (var prop in john) {
  console.log(prop + ": " + john[prop]);
}

// firstname: John
// lastname: Doe
// getFullName: function() {
//   return firstname + ' ' + lastname;
// }
```

## 判斷屬性是否為繼承 hasOwnProperty

那麼如果我們只希望取得該物件本身的 `properties` 該怎麼辦呢 ?

這時候便可以透過 `prototypal chain` 概念取得 `base object` 的方法 `hasOwnProperty` 來判斷該 `property` 是否來自於物件本身，是的話回傳 `true`，反之則回傳 `false`，因此我們就可以透過這個方法來篩選了。

```js
for (var prop in john) {
  if (john.hasOwnPorperty(prop)) {
    console.log(prop + ": " + john[prop]);
  }
}

// firstname: John
// lastname: Doe
```

## underscore.js extend()



接下來我們將透過 underscore.js 這個 libaries 的 source code 進一步了解 extend 的觀念。

讓我們來看一下透過 underscore.js 中的 `extend()` 可以做到什麼 ; 在載入 `underscrore.js` 的程式碼後，我們可透過下底線 `_` 來使用其中所有 API，在 `extned()` 這個 API 中，第一個參數是我們要擴充的物件，後續的參數則是擴充的來源，所以透過 `_.extend(john, jane, jim)` 便可讓 `john` 得到 `jane` 和 `jim` 的所有 properties。

```js
var john = {
  firstname: "John",
  lastname: "Doe",
};

var jane = {
  address: '111 Main St.'
};

var jim = {
  getFirstName: function() {
    return firstname;
  }
};

_.extend(john, jane, jim);

console.log(john);

// {
//   firstname: "John",
//   lastname: "Doe",
//   address: '111 Main St.',
//     getFirstName: function() {
//     return firstname;
//   }
// };
```

接下來讓我們到 [underscore.js extend source code](https://github.com/jashkenas/underscore/blob/master/underscore.js) 中，依序解析它的程式碼來了解它是如何辦到的。

首先，我們可以看到 extend 這個 API 來自於 `createAssigner` 這個函式，透過回傳一個函式這裡便形成一個 closures，以下依序為各個名詞的含意:

- `obj` : 要擴充的物件 `john`
- `keysFunc` : 取得物件中所有 `key` 的方法
- `length` : 傳入的物件數，如果只傳一個物件 `john` 則代表不擴充 `length < 2`，因此直接回傳
- 第 1 個 `for`: 從 `jane` 物件開始遍歷
- `keys` : 擴充來源物件 `jane` 和 `jim` 的所有 `key` 
- `l` : 擴充來源物件 `jane` 和 `jim` 的所有 `key` 的數量
- 第 2 個 `for`: 將擴充來源物件所有屬性指派給要擴充的物件 `john`

```js
// Extend a given object with all the properties in passed-in object(s).
var extend = createAssigner(allKeys);

function createAssigner(keysFunc, defaults) {
  return function (obj) {
    var length = arguments.length;
    if (defaults) obj = Object(obj);
    if (length < 2 || obj == null) return obj;
    for (var index = 1; index < length; index++) {
      var source = arguments[index],
        keys = keysFunc(source),
        l = keys.length;
      for (var i = 0; i < l; i++) {
        var key = keys[i];
        if (!defaults || obj[key] === void 0) obj[key] = source[key];
      }
    }
    return obj;
  };
}
```

以上便是透過解析 underscore.js 來了解 extend 概念，讓我們透過原型鍊 prototypal chain 的方式就可以擴充物件的屬性 ; 其實多閱讀這些知名 Library 的 source code 可以增進我們的程式撰寫技巧，讓我們用更聰明、簡潔的方式達到一樣的目的。

## 資料來源

[Udemy-JavaScript: Understanding the Weird Parts](https://www.udemy.com/course/understand-javascript/)`
