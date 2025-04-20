---
title: 強制型轉 Coercion
abbrlink: 50074
date: 2021-10-03 15:42:25
tags:
categories: 克服JS的奇怪部分
thumbnail:
banner:
description:
---

JavaScript 是一個動態型別的語言，當不同型別的參數透過運算子來運算時，JavaScript 會將參數的型別轉為相同，而不是回傳一個錯誤的訊息

<!-- more -->

上一章節中有提到透過加號 + 來做數字的運算，但其實不只能用在數字，在字串使用時會將彼此連結

```js
var a = 'Hello ' + 'World';   // Hello World
```

如果是一個數字加上字串的運算，JavaScript 會將數字的參數強制型轉成字串

```js
var b = '1' + 2;   // 12
```

即使這麼做不會出現錯誤，但一般來說盡量避免這樣的運算出現，原因是在開發時通常不會只有這麼簡單的運算，兩個參數之間可能有大量的程式碼，避免出現無法預期的 Bug

