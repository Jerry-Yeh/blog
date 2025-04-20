---
title: JaveScript入門篇 - 變數
tags: JavaScript
categories: JavaScript
abbrlink: b2bfbc85
date: 2019-09-26 17:52:20
thumbnail:
banner:
description:
---

變數為 JS 用來儲存資料和進行運算的基本單位，可以視為一個存放資料的盒子，透過 `var` 來宣告，`ES6` 開始可以使用 `let` 和 `const` 來宣告，關於 `ES6` 於之後的文章再作介紹

<!-- more -->

## 變數的規則

* 開頭不能是數字

  ``` js 
  var 3k = 30;
  ```

* 不能使用中線 `-` 和點 `.`

  ``` js
  var box-number = 30;
  var box.number = 30;
  ```

* 不能使用關鍵字

  [JS 關鍵字列表](https://www.w3schools.com/js/js_reserved.asp)

* 大小寫有區分，變數 `app` 不等於 `App`
* 盡量使用語意化命名，能透過名稱了解功能

  ``` js 
  var price = 30;
  var total = 100;
  ```

* 變數名稱是可以使用非英文(中文)的，但在其他語言系統可能會出現亂碼，所以還是盡量避免

## 變數的資料型別

變數本身沒有型別，變數的值才有，我們可以使用 `typeof` 運算子來判斷型別，型別分成基本型別 `string`、`number`、`boolean`、`null`、`undefined`，除了以上幾種型別，其他可以歸類為物件型別 `object`

``` js 
typeof(true);   // boolean
typeof('name');   // string
typeof(123);   // number
typeof({ });   // object
typeof([ ]);   // object

// 特例
typeof(window.alert);   // function
typeof(null);   //object
```

由以上結果可以看到 `function` 這個型別，但 `function` 其實不是一種獨立的型別，`function` 仍然是屬於 `object`
而 `null` 反而是不屬於 `object`，而是獨立的 `null` 型別，詳細可以參考以下資料

[Null and typeof](https://javascriptrefined.io/null-and-typeof-9330e475d272)
[The history of “typeof null”](https://2ality.com/2013/10/typeof-null.html)

* ### 數字 number

  分為整數和浮點數(有小數)，可以透過加 `+` 、減 `-`、乘 `*`、除 `/` 來做運算

  ``` js
  var a = 10;
  var b = 10.32;
  var c = 3 + 4;
  ```

* ### 字串 string

  * 字串必須用單引號 `''` 或雙引號 `""` 將其包住，而兩者不可混用

    ``` js
    var str1 = "字串1";
    var str2 = "字串2";
    var str3 = "字串3';  // error
    ```

  * 如果內容出現單引號 '，就只能用雙引號 `""` 包住，若非用不可，可以使用反斜線 `\` 跳脫字元 (escape character)

    ``` js
    var str = 'Let's go!';  // error
    var str = "Let's go!"; // OK
    var str = 'Let\'s go!';  // OK
    ```

  * 可以使用加號 `+` 來組合字串

    ``` js
    var str = 'Hello'+'World';
    ```

  * 多行字串可以透過反斜線 `\` 來換行繼續，注意 每行的反斜線 `\` 之後不能有任何東西，包括 `空白`

    ``` js
    var str = '第一行 \
    第二行 \
    第三行';
    ```
  
* ### 布林值 `boolean`

  分為 true 和 false 兩種，通常用在判斷式

  ``` js
  var a = true;
  var b = false;

  var c = (100 > 10);  //true 
  ```

* ### 空值 `null`

  代表曾經有值或沒有值，但現在此變數沒有值

* ### 未定義 `undefined`

  變數的宣告分成兩個步驟，"宣告" 與 "賦值"，當已經宣告變數但還未賦值時，就會出現 `undefined`

* ### 物件 `object`

  除了以上型別，其他都可以歸類為物件型別，關於物件型別後續會在介紹
