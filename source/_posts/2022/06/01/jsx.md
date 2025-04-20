---
title: JSX
abbrlink: 56348
date: 2022-06-01 23:14:32
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

在 React 中，我們可以透過將 HTML 直接寫在 JavaScript 來達到開發 UI 的目的，這種專屬格式就稱為 JSX

<!-- more -->

延續上一章的的內容，再透過 create-react-app 所建立的初始專案中，將 App.js 中的內容注入預設的 HTML 中達到第一個 Component 開發，在這個檔案中，我們可以直接在 function 中回傳 HTML 達到建立頁面的目的

```js
// App.js

function App() {
  return (
    <div>
      <h2>Let's get started !</h2>
      <p>This is also visible</p>
    </div>
  );
}

export default App;
```

而這種方式其實不存在於原生語法中，在原生語法中，我們需要透過以下幾行程式碼來建立一個 HTML 元件並賦值

```js
const para = document.createElement('p');
para.textContent = 'This is also visible';
document.getElementById('root').append(para);
```

而我們可以透過 JSX 方式來開發的原因就是 React 背景程式會將我們所開發的程式碼編譯成瀏覽器所能理解的原生語法，如果是在 Chrome 中，我們可以在透過 npm start 所建立的 local server 頁面的 devtools，打開 Application ，並看到 bundle.js，會發現也有一個 function App，但不只是我們開發的內容這麼簡單，而是包含了許多 React 程式碼，由此可見我們能這麼方便的開發，都是仰賴這些底層程式碼的編譯結果。

![](devtools.png)

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)