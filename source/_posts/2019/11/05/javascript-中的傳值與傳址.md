---
title: JavaScript 中的傳值與傳址
tags: JavaScript
categories: JavaScript
abbrlink: 30734
date: 2019-11-05 10:30:59
thumbnail:
banner:
description:
---

JavaScript 的變數再賦予不同型別的時候，會有同的傳遞方式，以 "基本型別" `Primitive` 和 "物件型別" `Object` 作為區分 

<!-- more -->

## 型別 Types

### 基本型別 Primitive type

當宣告兩個變數並賦予相同的基本型別時，兩個變數的會是 "相等" 的，這似乎很直觀

``` js
var a = 10;
var b = 10;
var c = "String";
var d = "String";
console.log( a === b );   // true
console.log( c === d );   // true
```

### 物件型別 Object type

但是當宣告兩個變數並賦予相同屬性與值的物件時，兩個變數卻是 "不相等" 的，因為 javascript 中的物件可以看成是個別的 "實體"，兩個物件雖然可以看成 "等值"，但卻不是同一個 "實體"

``` js
var obj1 = { value: 10 }
var obj2 = { value: 10 }
console.log( obj1 === obj2 );   // false
```

## 傳值與傳址

* 傳值 `pass by value` 
* 傳址 `pass by reference`

### 傳值 pass by value

在基本型別的變數中，當我們複製變數時，取得的是原先變數的 "值"，而不是這個變數本身

因為當指定 `b` 的值等於 `a` 時，會建立一個新的記憶體來存放相同的值，彼此不會互相干擾，這種情況稱為 "傳值" `pass by value` 

``` js
var a = 10;
var b = a;

console.log(a);   // 10
console.log(b);   // 10
```

![](傳值-1.png)

所以我們更改原先變數 `a` 的值時，並不會影響新變數 `b` 的值

``` js
a = 100;

console.log(a);   // 100
consolg.log(b);   // 10
```

![](傳值-2.png)

### 傳址 pass by reference

當我們建立一個物件型別的變數 a 時，一樣會建立一個位置在記憶體中;但是當我們另建立一個變數 b 並賦予他的值等於 a，這時和基本型別不一樣的是，並不會建立一個新的位置在記憶體中，而是讓變數 b 指向和 a 同一個記憶體中的位置，稱為 "傳址" `pass by reference`

``` js
var a = { value: 10 };
var b = a;

console.log(a);   // {value: 10}
console.log(b);   // {value: 10}
```

![](傳址-1.png)

所以當我們更改 `a` 的內容時，`b` 自然會跟著更改

![](傳址-2.png)

### 例外情況

有一些例外的情況透過傳值的方式複製物件，而複製又分為 "深度複製" `Deep copy` 和 "淺度複製" `Shallow copy`，"深度複製" 全部透過 "傳值" 的方式複製，而 "淺度複製" 只有 "第一層傳值"，內層還是 "傳址"

* 重新賦值

  凡事都有例外，當物件型別的變數被重新賦值時，就會在記憶體中產生一個新的位置來存放這個內容

  ``` js
  var a = {value: 10}
  var b = a;

  a = {value: 100}
  console.log(a);   // {value: 100}
  console.log(b);   // {value: 10}
  ```

  ![](傳值與傳址的例外.png)

* 手動賦值 (淺度複製)

  手動賦值很直接的把 "物件傳址" 改用 "傳值" 的方式傳遞，傳值的部分就會完全獨立 ; 但要注意的是，"內層物件" 還是 "傳址"

  ``` js
  var Person = {
    name: 'Jerry',
    bwh: {
      strength: 34,
      agility: 25,
      intelligence: 96
    },
  };

  Person2 = {
    name: Person.name, 
    bwh: Person.bwh,
  }

  Person.name = 'Tom';
  Person.bwh.strength = 0;

  console.log(Person2.name);   // 還是 "Jerry"
  console.log(Person2.bwh.strength);    // 一起改成 0  
  ```

  * for...in (淺度複製)

  透過 for...in 將原本的物件屬性的值傳到新的物件

  * 轉字串

  將物件轉型成字串，再將字串轉回物件，兩個物件會 "完全無關"

  ``` js
  var str = JSON.stringify(Person);
  var P = JSON.parse(Person);

  Person.name = 'Tom';
  Person.bwh.strength = 0;

  console.log(p.name);   // 還是 "Jerry"
  console.log(p.bwh.strength);    // 還是 34
  ```

  * jQuery

  jQuery 中可以透過 `extend()` 來複製物件，可以透過更改第一個參數來調整深度或淺度複製

  ``` js
  var p1 = jQuery.extend({}, Person);       // 淺度複製
  var p2 = jQuery.extend(true, {}, Person); // 深度複製

  Person.name = 'Tom';
  Person.bwh.strength = 0;

  console.log(p1.name);   // 還是 "Jerry"
  console.log(p1.bwh.strength);    // 改成 0
  console.log(p2.name);   // 還是 "Jerry"
  console.log(p2.bwh.strength);    // 還是 34
  ```

  * ES6

  在原生 JavaScript 中 ES6 也推出 `Object.assign()` 新的方法，但只適用於 "淺度複製"

  ``` js
  const p = Object.assign({}, Person);

  Person.name = 'Tom';
  Person.bwh.strength = 0;

  console.log(p.name);   // 還是 "Jerry"
  console.log(p.bwh.strength);    // 改成 0
  ```

## 資料參考

[[筆記] 談談JavaScript中by reference和by value的重要觀念](https://pjchender.blogspot.com/2016/03/javascriptby-referenceby-value.html)
[JavaScript 是「傳值」或「傳址」？](https://ithelp.ithome.com.tw/articles/10191057)