---
title: '框架小叮嚀:預設值 Framework Aside'
abbrlink: 19790
date: 2021-10-16 17:11:09
tags:
categories: 克服JS的奇怪部分
thumbnail:
banner:
description:
---

在開發 JavaScript 時，我們常會透過 `<script>` 來導入不同的套件，這章節來進一步了解套件被引入後的執行方式

<!-- more -->

一段常見的用法如下，我們透過 `<script>` 導入 `lib1.js` 和 `lib2.js` 這兩個套件，最後導入 `app.js` 來執行

```html
<html>
  <head>
  </head>
  <body>
    <script src="lib1.js"></script>  
    <script src="lib2.js"></script>  
    <script src="app.js"></script>  
  </body>
</html>
```

各自的程式碼如下

```js
// lib1.js
var libraryName = 'Lib 1';

// lib2.js
var libraryName = 'Lib 2';

// app.js
console.log(libraryName);   // Lib 2
```

結果會是 `Lib 2`，原因是 JavaScript 在執行這三段程式碼的時候，並不會為它們各自創造一個執行環境，直接地說是將彼此程式碼進行堆疊，所以實際上這兩個 Library 宣告了一個相同的全域變數，結果自然會以後者來取代。

所以如果在看許多套件的原始碼會看到類似以下的方式來宣告變數，避免影響到已存在的程式碼。

```js
// lib2.js
var libraryName = libraryName || 'Lib 2';

// 其實也就是在 window 宣告一個全域變數
window.libraryName = window.libraryName || 'Lib 2';
```

這麼一來後續導入的套件就不會影響到前者了

```js
var libraryName = libraryName || 'Lib 1';
var libraryName = libraryName || 'Lib 2';
console.log(libraryName);   // Lib 1
```
