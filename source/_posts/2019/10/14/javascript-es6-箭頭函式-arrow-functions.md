---
title: JavaScript - ES6 - 箭頭函式
tags:
  - JavaScript
  - ES6
categories: JavaScript
abbrlink: 33154
date: 2019-10-14 00:00:12
thumbnail:
banner:
description:
---

<!-- @format -->

`ES6` 開始可以使用箭頭 `=>` 來定義涵式，但與原先的函式在某些情況並不完全相同，在這篇文章中會探討箭頭函式 arrow function 的寫法與傳統函式的差異

<!-- more -->

## 基本寫法

- 正常寫法，省略 `function` ，使用箭頭 `=>`

  ```js
  var fnc = function (e) {
    return "Hello" + e;
  };
  var fnc = (e) => {
    return "Hello" + e;
  };
  ```

- 如果只有內容只有一行，可以省略 "大括號" `{ }` 和 `return`，並且自動回傳

  ```js
  var fnc = (e) => "Hello" + e;
  ```

- 如果只有 "一個參數"，可以省略 "小括號" `( )`

  ```js
  var fnc = (e) => "Hello" + e;
  ```

- 如果 "沒有參數" 或 "多個參數"，就一定要有 "小括號" `( )`

  ```js
  var fnc = () => "Hello";
  var fnc2 = (num1, num2) => num1 + num2;
  ```

## 沒有 arguments 參數

一般函式被呼叫時，都會產生一個 `argument` 物件，這個物件的內容就是我們帶入的 "參數"，而箭頭函式中是沒有 `arguments` 物件的

```js
var fnc = function (num1, num2) {
  console.log(arguments.length);
  return num1 + num2;
};

fuc(1, 2, 3, 4, 5); // 3
```

## this 差異

- 傳統函式 : `this` 的值是動態的，由呼叫這個函式的擁有物件 `object` 而定

- 箭頭函式 : `this` 的值綁定到其定義時所在的物件而定，也就是由外層的 function context 而定，如果外層沒有不存在任何 function context，就會指向最外層的 global context

  ```js
  var name = "我是全域";
  var test = {
    name: "我是作用域",

    callName: function () {
      console.log("1", this.name); // 1 我是作用域
      setTimeout(() => {
        console.log("2", this.name); // 2 我是作用域
        console.log("3", this); // 3 test 這個物件
      }, 100);
    },

    callName2: () => {
      console.log("4", this.name); // 4 我是全域
      setTimeout(() => {
        console.log("5", this.name); // 5 我是全域
        console.log("6", this); // 6 window 物件
      }, 100);
    },
  };

  test.callName();
  test.callName2();
  ```

  由以上範例可以看到，傳統函式 `function` 的 `this` 指的是這個函式的擁有物件 `test`，而箭頭函式在全域被宣告定義，所以其 this 自然指向最高級物件 window

  而外層的 "函式" 並不回影響箭頭函式的 `this` 原先指向的 `window`，外層的 "物件" 才會使箭頭函式的 `this` 指向這個物件

  ```js
  var func = function () {
    var func2 = function () {
      setTimeout(() => {
        console.log(this);
      }, 10);
    };
    var func3 = {
      func4: func2,
    };
    func2(); // window 物件
    func3.func4(); // func3 這個物件
  };
  func();
  ```

  由上範例可以看到，如果 "不是" 建立在 "物件" 內的箭頭函式，就不會影響箭頭函式的 `this`

  - func() : 最外層的函式，不會影響內部箭頭函式的 `this`
  - func2() : 第二層的函式，也不會影響內部箭頭函式的 `this`
  - func3() : 為一個物件，內部箭頭函式的 `this` 就會指向這個物件

## 特殊的情況

內建函式 `call`、`apply` 和 `bind`

```js
const obj = { a: 1 };

const func1 = function () {
  console.log(this);
};

const func2 = () => {
  console.log(this);
};

function func3() {
  setTimeout(function () {
    console.log(this);
  }, 1000);
}

function func4() {
  setTimeout(() => {
    console.log(this);
  }, 1000);
}

const func5 = () => {
  setTimeout(() => {
    console.log(this);
  }, 1000);
};

func1.call(obj); // obj
func2.call(obj); // window
func3.call(obj); // window
func4.call(obj); // obj
func5.call(obj); // window
```

- `func1` : 透過 `call` 呼叫給定 `this` ，所以 `this` 自然指向 `obj` 這個物件
- `func2` : 箭頭函式沒有宣告在物件內部，所以 `this` 還是指向 `window`
- `func3` : 傳統函式的 `this` 依據呼叫的方式，可以看成是 `window.setTimeout`，所以 `this` 指向 `window`
- `func4` : 因為外層 `func4` 的 `this` 被指定為 `obj`，所以 `setTimeout` 內的箭頭函式會在 `obj` 中被宣告
- `func5` : 因為外層 `func5` 在全域宣告，所以 `this` 自然指向 `window，setTimeout` 內箭頭函式的 `this` 也會指向 window`

### 不能用在建構式函式

由於箭頭函式的 `this` 是在其所定義的物件下建立，所以不能像 `function` 一樣作為建構式的函式，直接使用 `new` 會出現錯誤

```js
const test = (num) => {
  this.text = num;
};

const testAgain = new test("Hello World");
// Uncaught TypeError: test is not a constructor
```

### DOM 事件的監聽

在 `DOM` 事件監聽的情況下，函式的 `this` 應該指向這個 `DOM` 元素本身，但箭頭函式的 `this` 依然會指向定義其所在的物件 `window`，所以不適用於這類情況

```js
var btn = document.querySelector(".btn");
var addSomething = () => {
  console.log(this); // window 物件
  this.innerHTML = "AddSomething"; // Error
};

btn.addEventListener("click", addSomething);
```

## 使用建議

- 物件內的函式可以優先使用，因為傳統函式以往的做法會先將 `this` 賦予再另一個變數上，以記錄當下的 `this`

```js
var test = {
  callName() {
    var that = this;
    setTimeout(function () {
      console.log(that); // test 這個物件
    }, 1000);
  },
  callName2() {
    setTimeout(() => {
      console.log(this); // test 這個物件
    }, 1000);
  },
};
```

- 雖然只有一個參數時可以省略 "小括號" `( )`，但還是建議都加上，避免意外錯誤

## 資料參考

[你不可不知的 JavaScript 二三事#Day21：箭頭函數 (Arrow Functions) 的 this 和你想的不一樣 (1)](https://ithelp.ithome.com.tw/articles/10207992)
