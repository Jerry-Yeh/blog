---
title: 物件與點 Objects and the Dot
abbrlink: 22552
date: 2021-10-20 11:59:21
tags:
  - 物件與函式
categories: 克服JS的奇怪部分
thumbnail:
banner:
description:
---

這篇文章中我們將會探討什麼是 JavaScript 中的`物件 (Object)`

<!-- more -->

## 什麼是物件 Object ?

在 JavaScript 中，物件其實就是一對`名稱/值的配對 (key value pairs)`，而值又可以是另一組名稱/值的配對

![](key-value-pairs.PNG)

物件的值又可以方為以下三種:

- 純值 Primitive
  第一種是原生的值，像是布林值 (Boolean)、數值 (Number) 和字串 (String)，在物件當中，我們把這類的值稱為物件的屬性 (Property)
- 物件 Object
  第二種是物件 (Object)，沒有錯，就像一開始說的物件中也可以再放入物件，也就是另一組名稱/值的配對，就像是孩子一樣，這一類的值我們也稱它會為物件的屬性 (Property)
- 函數 Function
  第三種值是函數，在物件中的函數我們會將它稱為物件的方法 (Method)

當 JavaScript 宣告一個物件時，這個物件會在記憶體中佔有一個位置，物件中的屬性 (Property) 和方法 (Method) 也會，而物件會記錄其屬性和方法的位置使其可以參考到，這些屬性和方法的位置彼此可能有關也可能無關

## 建立物件與屬性

### 方法一: 中括號 [] (Bracket)

首先透過 `new` 這個關鍵字來建立一個物件，並透過中括號 [] (Bracket) 來新增物件的屬性，而中括號其實也是一個運算子，我們可以透過它使物件參考到對應屬性的值

![](Bracket.PNG)

```js
var person = new Object();

person['firstname'] = 'Tony';
person['lastname'] = 'Alicea';
```

這個運算子好用的地方是也可透過變數來儲存物件屬性的名稱

```js
var firstNameProperty = 'firstname';

console.log(person[firstNameProperty]);   // Tony
```

### 方法二: 點 . (Dot)

第二種方法更常見於開發，當我們透過點 .(Dot) 來指定物件的屬性時，可以省略引號直接當成字串搜尋物件

```js
console.log(person.firstname);   // Tony
console.log(person.lastname);    // Alicea
```

當然我們也可以透過點 . 來新增一個值為物件的屬性，且可以不斷新增其子屬性

```js
person.address = new Object();
person.address.street = '111 Main St.';
person.address.city = 'New York';
person.address.state = 'NY';

console.log(person.address.street);   // 111 Main St.
console.log(person.address.city);     // New York
console.log(person.address.state);    // NY
```

## 補充

[作者](https://www.udemy.com/user/anthonypalicea/) 最後也提到，雖然中括號 [] (Bracket) 和點 . (Dot) 都可以建立、讀取物件屬性，但一般來說會建議使用點 . 來建立，它可以使程式碼更簡潔、更好除錯，除非需要使用動態字串來取得屬性。
另外[作者](https://www.udemy.com/user/anthonypalicea/) 也提到，使用 `new` 關鍵字來建立物件並不是一個好方法，這在之後的課程會再介紹。