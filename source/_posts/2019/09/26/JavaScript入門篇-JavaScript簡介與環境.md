---
title: JavaScript入門篇 - JavaScript簡介與環境
tags: JavaScript
categories: JavaScript
abbrlink: e0d364c
date: 2019-09-26 15:22:59
thumbnail:
banner:
description:
---

作為前端領域最強勢的語言，`JavaScript` 隨著新版本演進，功能日趨完善，也不斷出現如 `jQuery` 與 `Vue.js` 等開源框架，讓 `JavaScript` 成為學習前端不可或缺的技能 ; 在這篇文章中，將透過 `VSCode` 來做為開發的編輯器，並部屬基礎環境設定

<!-- more -->

## JavaScript 環境設定

有時候我們會利用 JavaScript 新增 HTML 樣板到 HTML 檔案中，而 VSCode 預設在 JavaScript 檔案中是無法使用 `Emmet` 的，因此我們要在 VSCode 的設定中的 `setting.json` 加上以下文字

``` json 
// in setting.json 
"emmet.includeLanguages": {
  "javascript": "html"
}
```

## 建構一個 JavaScript 環境

如同 CSS 使用 `<link>` 載入 `.css` 檔案或使用 `<style></style>` 在內部直接撰寫 CSS，JavaScript 是使用 `<script></script>`，而又因為 HTML 是 "由上而下" 讀取，JS 時常會操作當中的 `DOM 元素`，所以 JS 要放在 "網頁內容之後" 以避免出現目標元素還未渲染而造成讀取錯誤的情況

``` html  
<body>
  <div class="content">
    <!-- 網頁內容 -->
  </div>
          
  <!-- 載入 .js 檔案 --> 
  <script src="js/all.js"></script>

  <!-- 直接撰寫 JS --> 
  <script>
    alert('Hello World!');
  </script>
</body>
```

## HTML、CSS、JavaScript 的關聯性

* `HTML` - 內容 : HTML 負責網頁的內容，為網頁的骨架
* `CSS` - 樣式 : CSS 負責網頁的樣式，替網站添加裝飾
* `JavaScript` - 行為 : JavaScript 負責網頁的行為，可以動態操作 HTML、CSS，也可以連接外部的伺服器與資料庫，與使用者互動

## ID 與 textContent

### ID

* `ID 選擇器` 具備 "錨點功能"，也就是能定位在網頁上，如果指定到該錨點，會在網址後方加上該 `ID` 名稱

  ``` 
  /* 選定前網址 */ 
  https://jerry-yeh.github.io/website/html/JavaScript/JavaScript-入門篇.html

  /* 選定後網址 */ 
  https://jerry-yeh.github.io/website/html/JavaScript/JavaScript-入門篇.html#ID
  ```

* 在 JS 中可以透過以下語法選取 ID

  ``` js
  document.getElementById('ID 名稱').textContent('修改後的內容');
  ```

### textContent

在 JS 中可以透過此語法選取 `DOM 元素` 的內容

``` js
document.getElementById('ID 名稱').textContent('修改後的內容');
```

## 直譯器流程介紹

瀏覽器接收到網頁後，就會由上而下開始解析網頁內容，當解析到 JS `<script></script>` 的部份時，會透過瀏覽器的引擎來將我們寫的 JS 程式語言轉換成電腦認識的語言，而這中介的引擎稱為直譯器 `interpreter`，如 `Chrome` 的 `V8` 引擎，而解析完後瀏覽器才會繼續渲染網頁內容

## 資料參考

[六角課程 - JavaScript 入門篇 - 學徒的試煉](https://www.udemy.com/javascript-learning/)
[JavaScript Wiki](https://zh.wikipedia.org/wiki/JavaScript)