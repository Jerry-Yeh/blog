---
title: Log 雖可恥但有用
abbrlink: 34642
date: 2021-10-20 20:01:42
tags: 
  - Debug
categories: Browser
thumbnail:
banner:
description:
---

紀錄一下前端開發時常使用到的 `console.log` 所遇到的小陷阱

<!-- more -->

在前端開發時，我們常會使用到 `console.log` 來印出資料內容以確保程式碼實行的結果與我們預期的一致

```js
let arr = [2. 7, 5];
console.log(arr);   // [2, 7, 5]
```

但當我們執行一對程式碼後，但這個 Array 進行修改後，會發現即使在修改前使用 `console.log` 印出資料，點擊後查看結果仍會是修改後的資料

```js
let arr = [2. 7, 5];
console.log(arr);
// 一堆程式碼...
arr.push(3);
```

![](log.PNG)

原因是瀏覽器在印出 `log` 時，會印出最終資料，這裡我們可以透過將資料轉成字串 `JSON.stringify()` 來呈現資料當下的狀況

```js
let arr = [2. 7, 5];
console.log(JSON.stringify(arr));   // [2, 7, 5]
// 一堆程式碼...
arr.push(3);
```

