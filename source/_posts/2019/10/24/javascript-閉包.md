---
title: JavaScript - 閉包 Closure
tags: JavaScript
categories: JavaScript
abbrlink: 33578
date: 2019-10-24 00:40:03
thumbnail:
banner:
description:
---

閉包 `Closure` 是 JavaScript 中一種透過函式包住函式的技巧，透過這種技巧可以避免函式中的變數汙染到全域，而出現相同變數名稱互相干擾的情況

<!-- more -->

## 範圍鏈 Scope Chain

了解閉包前，先來了解範圍鏈的概念 ; 切分變數有效範圍的最小單位是 `function`，所以當 `function` 在自己的層級找不到變數時，就會一層一層往外找，直到全域範圍 `Global Environment`，這種行為就稱為 "範圍鏈" `Scope Chain`

從以下範例可以看到在當個層級中找不到變數，所以一層一層往外找，直到全域範圍

``` js
var a = 1;
funcion outer (){
  var b = a * 2;

  function inner (c){
    console.log(a, b, c);   // 1 2 6
  }
  inner(b * 3);
}

outer();
```

## 閉包 Closure

先了解範圍鏈的原因在於充分理解作用域之後，就能理解為什麼要使用閉包的概念

### 什麼是閉包 ?

* 呼叫函式中的函式
* 內層函式中的變數不會汙染到全域
* 內層函式的變數可以在內部重複使用，不被釋放到全域
* 內層函式可以記錄當時的變數值

### 沒有使用閉包的情況

以下透過常見的 "計數器" 來了解閉包

``` js
var count = 0;

function count(){
  return ++count;
};

console.log( count() );   // 1
console.log( count() );   // 2
console.log( count() );   // 3
```

一個全域變數的宣告似乎沒什麼問題，但是當多人協作或是自己在多行程式碼後又建立一個計數器來記錄時，就容易發生變數名稱相同的衝突

``` js
var count = 0;

function countDog(){
  return ++count;
};

// 多行程式碼後

var count = 0;

function countCat(){
  return ++count;
};

console.log( countDog() );   // 1
console.log( countDog() );   // 2
console.log( countCat() );   // 3   預期是 1 
```

### 閉包寫法

透過閉包的概念，使上述範例中各自的變數 `count` 成為區域變數，並將真正要執行的函式放到內層，藉由 `return` 回傳後呼叫外層函式

``` js
function dogsHouse (){
  var count = 0;
  function countDogs (){
    count += 1;
    console.log( count + 'dogs' );
  };
  return countDogs
}

function catsHouse (){
  var count = 0;
  function countCats (){
    count += 1;
    console.log( count + 'cats' );
  };
  return countCats
}

const dogCounter = dogsHouse();
const catCounter = catsHouse();
dogCounter();   // 1 dogs
dogCounter();   // 2 dogs
catCounter();   // 1 cats
catCounter();   // 2 cats
```

### 進一步使用閉包

即使將函式賦予不同的宣告變數，彼此都是獨立執行不會互相干擾

``` js
function dogsHouse (){
  var count = 0;
  function countDogs (){
    count += 1;
    console.log( count + 'dogs' );
  };
  return countDogs
}

const l = dogsHouse();
const m = dogsHouse();
const s = dogsHouse();

l();   // 1dogs
l();   // 2dogs
m();   // 1dogs
m();   // 2dogs
s();   // 1dogs
s();   // 2dogs
```

### 將參數帶入閉包

還可以將參數帶入閉包，使每個變數回傳不同結果

``` js
function dogsHouse (s){
  var count = 0;
  function countDogs (){
    count += 1;
    console.log( count + ' ' + s );
  };
  return countDogs
}

const l = dogsHouse('l');
const m = dogsHouse('m');
const s = dogsHouse('s');

l();   // 1 l
l();   // 2 l
m();   // 1 m
m();   // 2 m
s();   // 1 s
s();   // 2 s
```

### 簡化程式碼

在閉包運用中，都會回傳 `return` 內層的函式，這時就可以簡化成 `return` 後面直接接上 "匿名函式"

``` js
function dogsHouse (s){
  var count = 0;
  
  return function (){
    count += 1;
    console.log( count + ' ' + s );
  };
}

function catsHouse (s){
  var count = 0;
  
  return function (){
    count += 1;
    console.log( count + ' ' + s );
  };
}
```

而透過上方的 "進一步使用閉包"，將函式賦予到不同變數上，再透過不同參數加以區分

``` js
function counter(s){
  var count = 0;

  return function (){
    count += 1;
    console.log( count + '' + s );
  }
}

var dogCounter = counter('dogs');
var catCounter = counter('cats');
dogCounter();   // 1 dogs
catCounter();   // 1 cats
dogCounter();   // 2 dogs
catCounter();   // 2 cats
```

### ES6 寫法

透過 ES6 使函式更精簡

``` js
let counter =  (s) => {
  let count = 0;
  return () => {
    count += 1;
    console.log( count + " " + s );
  }
}
```

## 參考資料

[重新認識 JavaScript: Day 19 閉包 Closure](https://ithelp.ithome.com.tw/articles/10193009)
[另一種方式介紹 JavaScript 閉包](https://ithelp.ithome.com.tw/articles/10191515)
[深入淺出瞭解 JavaScript 閉包（closure）](https://pjchender.blogspot.com/2017/05/javascript-closure.html)