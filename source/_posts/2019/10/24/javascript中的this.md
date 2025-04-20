---
title: 'JavaScript 中的 this'
abbrlink: 42922
date: 2019-10-24 00:23:43
tags: JavaScript
categories: JavaScript
thumbnail:
banner:
description:
---

- `this` 是 JavaScript 中的一個關鍵字
- `this` 是 `function` 執行時，自動生成的一個內部物件
- 隨著執行的場合不同， `this` 所指向的值也會有所不同
- 影響 `this` 的是 "函式的呼叫方法"，而非函式宣告的時機

<!-- more -->

大多數情況 `this` 就代表 "呼叫函式 `function` 的物件"

## 直接呼叫

如果是直接呼叫函式，此函式中的 `this` 都會指向 `window`，即使是在 `function` 中的 `function` 也是

``` js
window.name = '我是全域'
function callName(){
  console.log(this.name);   // '我是全域'
  
  function callName2(){
    console.log(this.name);   // 我是全域
  }
  callName2();
}
callName();
```

## 物件呼叫

如果函式 `function` 是在物件下呼叫，函式中的 `this` 就會指向該物件，無論 `function` 是在那裡宣告

``` js
var name = '我是全域';
function callName(){
  console.log(this.name);   // ''
};

var test = {
  name: '我是區域',
  callName2: callName
}

callName();         // '我是全域' 
test.callName2();   // '我是區域
```

如果將物件內的 `function` 賦予到變數上並呼叫，這個 `this` 就會指向全域

``` js
callName3 = test.callName2;
callName3();    // '我是全域'
```

## DOM 元素呼叫

透過 DOM 元素的事件監聽來呼叫 `function`，此 `function` 中的 `this` 就會指向該 DOM 元素

``` html
<p class="test">我是 DOM 元素</p>
```

``` js
var list = document.querySelector('.list');

function test(){
  console.log(this);   // <p class="test">我是 DOM 元素</p>
};
              
list.addEventListener('click', test);
```

## 建構式呼叫

在建構式下會使用 `new` 來生成一個新物件( 物件被建構出來 )，而 `function` 中的 `this` 會指向這個物件，而且可以在生成的物件上重新定義

``` js
var name = '我是全域';
function callName (newName){
  this.name = newName || '我是區域';
};

var callName2 = new callName('我是新的區域');

var callName3 = new callName();
console.log(callName2.name);   // 我是新的區域
console.log(callName3.name);   // 我是區域
```

## 內建函式來呼叫

透過內建函式 `call`、`bind`、`apply` 來呼叫函式，第一個參數會成為這個函式的 `this`，只是這三個內建函式在使用時機上有所不同

``` js
var name = '我是全域';
function callName(){
  console.log(this.name);
};

callName();   // '我是全域'
callName.call({name: '我是區域'});   // '我是區域'
```

## 重新指向 this

在實際操作時，遇到 "立即函式" IIFE 或 "非同步事件" 如 `setTimeout`，`this` 大多會指向 全域 `window`，如果要調用的是物件本身，可以先設定一個變數指向這個物件，再將這個變數帶入函式中

常見的變數名稱有 `that`、`vm`、`self` 等

``` js
var name = '我是全域';
function callName(){
  console.log(this.name);     // 我是區域
  var that = this;
  setTimeout(function (){
    console.log(this.name);   // 我是全域
    console.log(that.name);   // 我是區域
  }, 1000);
};

var test = {
  name: '我是區域',
  callName2: callName
}

test.callName2();
```

## 結論

- 如果 `function` 是透過物件來呼叫，那麼其中的 `this` 就會指向該物件
- 如果 `function` 是透過 DOM 元素的事件監聽來呼叫，那麼其中的 `this` 就會指向該 DOM 元素
- 如果 `function` 是透過建構式 `new` 生成的物件來呼叫，那麼其中的 `this` 就會指向該物件
- 如果 `function` 是透過內建函式 `call`、`apply`、`bind` 來呼叫，那麼其中的 `this` 就會指向內建函式的第一個參數
- 如果沒有滿足以上條件，那麼 `function` 的 `this` 就會指向全域物件 `window`

## 參考資料

[What's "THIS" in JavaScript](https://ithelp.ithome.com.tw/articles/10193193)
[JavaScript 的 this 到底是誰？](https://ithelp.ithome.com.tw/articles/10191393)