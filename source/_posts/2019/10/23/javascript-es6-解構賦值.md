---
title: JavaScript - ES6 - 解構賦值 deconstructing
abbrlink: 13442
date: 2019-10-23 10:07:59
tags:
  - ES6 
  - JavaScript
categories: JavaScript
thumbnail:
banner:
description:
---

過去陣列和物件內的元素在賦值時，都是透過一一給值的方式達成，在 ES6 中，可以透過解構的方式一次賦值，有效減少程式碼

<!-- more -->

## 陣列解構賦值 Array deconstructing

以往陣列賦值都是直接給值如下

``` js
let onePiece = ["魯夫", "索隆", "娜美", "香吉士", "騙人布"];
let captain = onePiece[0];
let fighter = onePiece[1];

console.log(captain);   // 魯夫
```

### ES6 一般用法

``` js
let [captain, fighter, navigator, chef, sniper] = onePiece;

console.log(captain);   // 魯夫
```

### 輸入的變數量 "少於" 給得值或遇到空變數

如果輸入的變數量 "少於" 給得值或遇到空變數，只有指定到的變數有值，其餘都會直接跳過

``` js
let [captain, fighter, , chef] = onePiece;

console.log(fighter);   // 索隆
console.log(chef);   // 香吉士
```

### 輸入的變數量 "多於" 給得值

而如果輸入的變數量 "多於" 給得值就會賦予 `undefined`

``` js
let [captain, fighter, navigator, chef, sniper, doctor] = onePiece

console.log(doctor);   // undefined
```

### 賦予預設值

在輸入的變數中也可以賦予預設值，如果變數沒有取得對應的值，就會直接帶入預設值

``` js
let [captain = '從缺', fighter, navigator, chef, sniper, doctor = '喬巴'] = onePiece

console.log(captain);   // 魯夫
console.log(doctor);   // 喬巴
```

### 互換變數

傳值是有順序性的，且傳遞是及時的，不需要第三方變數來取代，所以可以直接透過解構賦值的方式互換變數順序以交換變數值

``` js
let captain = '魯夫';
let fakeCaptain = '迪馬爾·布拉克';
          
[captain, fakeCaptain] = [fakeCaptain, captain];

console.log(captain);   // 迪馬爾·布拉克
```

### 賦予字串

如果解構方法所賦予的值是字串時，就會將字串一一拆解成單一字元賦予到變數上

``` js
let promise = "3D2Y";

let [three, day, two, year] = promise;

console.log(three);   // 3
console.log(year);   // Y
```

## 物件解構賦值 Object deconstructing

物件解構和陣列解構有相同的解構概念，不同的地方在於對應的值，陣列解構是透過索引的 "順序" 對應，而物件解構是透過 "屬性名稱" 來對應

``` js
let onePiece = {
  captain: '魯夫',
  fighter: '索隆'
}

let {captain, fighter} = onePiece;

console.log(captain);   // 魯夫
```

要注意的是新的物件屬性名稱必須和原物件屬性名稱相同，因為以上寫法其實是屬性和值的名稱相同時的 "縮寫"

``` js
let {captain: captain, fighter:fighter}
```

冒號 `:` 前的屬性名稱只是對應，真正賦值的是冒號 `:` 後的值，從改變值的名稱就可以了解其中概念

``` js
let {captain: cap, fighter: fight} = onePiece;

console.log(captain);   // Error
console.log(cap);   // 魯夫
```

### 輸入的變數量與給得值 "不同" 或遇到空變數

與陣列不同的是，因為物件是採屬性名稱對應而不是順序對應，所以遇到空變數會出現錯誤 `Error`，變數量與給得值也沒有數量對應問題，沒有對應到相同屬性就是 `undefined`

``` js
let {captain, , navigator} = onePiece;   // Error

console.log(navigator);   // undefined
```

### 賦予預設值

同樣的物件解構也可以賦予預設值

``` js
let {captain = '魯夫', fighter} = {fighter: '索隆'};

console.log(captain);   // '魯夫'
console.log(fighter);   // '索隆'
```

## 應用

以下介紹幾個在 ES6 解構賦值常使用到的情境

### 指定參數

解構賦值有許多實際的應用，若將其運用到函式參數中，就能 "指定" 要帶入的參數而不是直接按照順序帶入

``` js
function battle({fighter1 = '魯夫', fighter2 }){
  console.log(`${fighter1} k.o ${fighter2}`);
};

battle({});   // 魯夫 k.o undefined
battle({fighter2: '大仔'});   // 魯夫 k.o 大仔
battle({fighter2: '龍馬', fighter1: '索隆'});    // 索隆 k.o 龍馬
```

### 解構賦值搭配縮寫與展開

在前面提到，如果直接將物件賦予到新物件上，是透過 "傳址" 的方式複製，如果改變其一，兩者都會更改

``` js
const onePiece = {
  captain: {
    name: '魯夫'
  },
  fighter: {
    name: '索隆'
  }
}

const newTeam = onePiece;
newTeam.navigator = {name: '娜美'}

console.log(onePiece.navigator.name);   // '娜美'
```

這時候就可以透過 "解構賦值" 與 "縮寫" 的方式賦予新物件

``` js
const newTeam = {
  onePiece
}
```

但這樣的結果形式和原來的不一樣，因為是將物件直接帶入，所以內容是物件 onePiece 和 變數 navigator，所以要透過展開 `...` 將物件內容釋放出來

``` js
const newTeam = {
  ...onePiece
}
```

其他應用可以參考以下連結

[变量的解构赋值](http://es6.ruanyifeng.com/#docs/destructuring)