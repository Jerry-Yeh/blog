---
title: A Closer Look at JSX
abbrlink: 55243
date: 2022-06-05 14:22:55
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

在前面關於 React 的基礎介紹中，我們反覆提到 React Component 中的 function return 的 JSX 並非瀏覽器所能讀取的 HTML，只是經過 React 封裝過看起來像是 HTML 而已，接下來我們會進一步探討 JSX 是如何運作的

<!-- more -->

首先，在透過 `npm start` (or `yarn start`) 所開啟的網頁中，打開 devtools -> Application 中，可以看到在 bundle.js 內也有一個 function APP，這就是實際經過 React 編譯後，能被瀏覽器所讀取的最終結果。

![](devtools.png)

## React.createElement()

接著回到我們透過 create-react-app 所建立的專案中，可以從 package.json 中知道預設安裝了 react 和 react-dom 這兩個 React 套件，但目前的 React 版本 (18) 的 Component 中，我們並不需要 import react，原因是 React 已經在背景為我們做到這件事了，這在較早的 React 版本中是需要開發者自行引入的，這裡我們可以嘗試使用舊有的方式來改寫 JSX。

以根目錄 App.js 為例，我們先 import react 物件，再透過 `React.createElement` 這個方法建立 HTML element，其中的參數分別為以下內容:

- 第一個參數: 要建立的 HTML tag，如果是自製 Component 則直接帶入物件
- 第二個參數: tag 上的屬性
- 第三個參數: 這組 element 中的內容，如果是另外一層 HTML tag，則再透過 `React.createElement` 建立

經過以上方式改寫後，結果就會如下方範例程式碼，且可以達到完全相同的結果

```js
// App.js
import "./App.css";
import Expenses from "./components/Expenses";
import React from "react";

function App() {
  const expenses = [
    { title: "Car Insurance", amount: 294.67, date: new Date(2022, 6, 4) },
    { title: "Toilet Paper", amount: 94.12, date: new Date(2022, 6, 3) },
    { title: "New TV", amount: 299.49, date: new Date(2022, 6, 2) },
    { title: "New Desk (Wooden)", amount: 450, date: new Date(2022, 6, 1) },
  ];

  return React.createElement(
    "div",
    {},
    React.createElement("h2", {}, "Let's get started!"),
    React.createElement(Expenses, { items: expenses })
  );

  // return (
  //   <div className="App">
  //     <h2>Let's get started!</h2>
  //     <Expenses items={expenses} />
  //   </div>
  // );
}

export default App;
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[Github](https://github.com/Jerry-Yeh/react-complete-guide/commit/f2bd63783737eb58e311d7ecd36d476baaebc7d8)
