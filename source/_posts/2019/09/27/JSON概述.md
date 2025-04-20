---
title: JSON概述
tags: JSON
categories: JSON
abbrlink: b2a33033
date: 2019-09-27 02:32:22
thumbnail:
banner:
description:
---

JSON 常用來作為資料傳遞的檔案格式，這篇文章針對 JSON 做一個簡單的概述

<!-- more -->

## 資料來源

[JSON Wiki](https://zh.wikipedia.org/wiki/JSON)
[Day13– 前端小字典三十天【每日一字】– JSON](https://ithelp.ithome.com.tw/articles/10159700)
[高雄市政府資料開放平台](https://data.kcg.gov.tw/)
[行政院環境保護署](https://opendata.epa.gov.tw/Data/Contents/RainTenMin/)

## JSON 是什麼 ?

* `JSON` 是一種輕量級的 資料交換語言，但其並非一種新的程式語言，而是一種 資料傳輸的格式
* 透過 JS 的物件 `Object {}` 和 陣列 `Array []` 所構成
* 目前很多程式語言都可以支援 `JSON` 的格式，其附檔名為 `.json`

## JSON view chrome 插件

如果是 chrome 的使用者，可以下載 `JSON view` 這個插件，它能將 `JSON` 檔案整理成易讀的格式

[JSON view](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?hl=zh-TW)

## JSON 的載入方式

將未經 `JSON view` 處理過的格式複製下來( 未經插件處理過的資料在載入 JS 中比較不會出問題 )，存入到一個變數中，就可以透過物件、陣列的擷取方式來擷取資料

[範例程式碼](https://codepen.io/Jerry-Yeh/pen/ydRqNy?editors=0010)
