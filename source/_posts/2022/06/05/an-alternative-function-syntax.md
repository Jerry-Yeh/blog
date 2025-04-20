---
title: An Alternative Function Syntax
abbrlink: 34888
date: 2022-06-05 15:02:48
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

在 React Componet 中，有一點反覆提到的就是其實我們就是在寫 JavaScript，所以我們當然也可以透過其他 JavaScript 語法來進行改寫，如 ES6 Arrow Function 就可以直接改寫 Compoent return function，但這並非必要，只是更進一步說明任何 JavaScript 語法都可以直接運用在 React Component 之中。

<!-- more -->

```js
// App.js

function App() {...};

// ES6 Arrow Function
const App = () => {...};
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[Github](https://github.com/Jerry-Yeh/react-complete-guide/commit/8f79a87176b7da2e1b54521a00c619476b404baa)