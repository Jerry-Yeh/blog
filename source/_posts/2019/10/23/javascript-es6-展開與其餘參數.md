---
title: JavaScript - ES6 - 展開與其餘參數
tags:
  - ES6
  - JavaScript
categories: JavaScript
abbrlink: 13950
date: 2019-10-23 09:59:03
thumbnail:
banner:
description:
---

在 ES6 中增加了一個新的方法來展開陣列中的值，這個方法就是展開 `...`

<!-- more -->

## 展開 Spread ...

如下範例，我們將一班的陣列在前方加上 `...` 後，陣列的資料內容就會被一一展示開來 

``` js
let groupA = ['魯夫', '索隆', '娜美'];
let groupB = ['香吉士', '騙人布'];

console.log(...groupA);   // 魯夫 喬巴 索隆
```

透過展開這個方法，我們還可以連接陣列 ; 順便比較 ES6 以前的方法

``` js
const onePiece = [...groupA, ...groupB];
const onePiece2 = groupA.concat(groupB);

console.log(onePiece);   // ["魯夫", "索隆", "娜美", "香吉士", "騙人布"]
console.log(onePiece2);   // ["魯夫", "索隆", "娜美", "香吉士", "騙人布"]
```

### 淺層複製 Shallow copy

一般來說因為陣列也是物件，所以直接賦值會透過 "傳址" 的方式來傳遞，而展開方法是一種 "淺層複製" shallow copy，因為是將原陣列先拆開再放入新陣列中，所以是透過 "傳值" 的方式複製第一層資料

``` js
let groupA = ['魯夫', '索隆', '娜美'];
let groupB = groupA;
ler groupC = [...groupA]

groupA.push('香吉士');

console.log(groupA);   // ["魯夫", "索隆", "娜美", "香吉士"]
console.log(groupB);   // ["魯夫", "索隆", "娜美", "香吉士"]
console.log(groupC);   // ['魯夫', '索隆', '娜美']
```

### 類陣列 Array-like 轉陣列 Array

在 JavaScript 中，有許多有著陣列外觀卻無法使用所有陣列方法的物件我們稱為 "類陣列" `Array-like`，類陣列並不與一般陣列共用同一個原型，常見的類陣列如 `DOM 陣列` 和 `Argument` 

``` js
let doms = document.querySelectorAll('p');

function arg(){
  return arguments;
};

console.log(arg(1, 2, 3));   // Arguments(3) [1, 2, 3]
```

這些類陣列 `Array-like` 可以透過 "展開" `...` 轉為真正的陣列

``` js
let spreadDoms = [...doms];
let spreadArg = [...arg(1, 2, 3)];

console.log(spreadArg);   // [1, 2, 3]
```

## 其餘參數

如果帶入的參數數量大於預設的數量，可以透過展開 `...` 的方式將多餘的參數集合到陣列中

``` js
function onePiece(caption, ...crew){
  console.log(caption, crew);
}

onePiece('魯夫', '索隆', '娜美');

// in console
'魯夫' ['索隆', '娜美']
```