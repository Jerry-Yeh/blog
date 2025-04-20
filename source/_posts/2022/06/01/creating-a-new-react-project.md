---
title: Creating a new React Project
abbrlink: 19798
date: 2022-06-01 08:18:27
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

<!-- @format -->

Create React App 是創建 React 專案最方便的方式，對於我一個過去為 Vue 的開發者來說，他就如同 Vue CLI 的存在 ; 同樣也以 Node.js 作為背後的驅動，並在開發時提供一個 local server 及時同步開發程式與網頁呈現，並在最後將這些 Framework 的專屬程式碼打包成瀏覽器看得懂的原生 HTML、CSS 和 JavaScript 部屬在 production 環境。

<!-- more -->

## 建立專案

首先，我們需要先要先依據各自的作業系統安裝 [Node.js](https://nodejs.org/zh-tw/download/) 作為背後的驅動，接著可以在 [Github](https://github.com/facebook/create-react-app) 或 [官方網站](https://create-react-app.dev/) 依照步驟安裝 Create React App

### npx

```bash
npx create-react-app ${app name}
```

### npm

```bash
npm init react-app ${app name}
```

### yarn

```bash
yarn create react-app ${app name}
```

接著進到專案資料夾內，透過以下指令安裝 npm 預設第三方套件環境

```bash
npm install
```

最後可以透過 npm 或 yarn 來啟動專案

```bash
cd ${app name}

yarn start
or
npm start
```

## 資料夾結構

透過 create-react-app 所建立的 React 專案架構和 Vue CLI 所建立的專案基礎架構很類似，同樣都會有 node_modules 和 package.json 來管理第三方套件，並於 src 資料夾中進行開發

```
node_moduels
public
  - index.html
src
  - index.js
  - index.css
  - App.js
package.json
package-lock.json
.gitignore
```

在 React 專案中，有別於 Vue 中有 .vue 這種特殊的檔案類型，React 其實就是在寫 JavaScript，但又會發現許多原生以外的語法，例如在初始 index.js 中，就會引入 index.css 和 App.js，但其實原生檔案根本不會把 CSS 引入 JavaScript 中，原因是 React 環境會幫我們把這些編譯成瀏覽器可以解讀的語法，並透過 npm start 啟動一個 local server，及時同步開發內容

```js
// index.js
import "./index.css";
import App from "./App";
import React from "react";
import ReactDOM from "react-dom";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<App />);
```

接著往下看，會看到引入了來自 react 和 react-dom 的物件，這兩個套件其實都是 React ; `ReactDOM.createRoot` 透過 id 取得一個名為 root 的 HTML 元件，這時候可以看到 public 內的 index.html 中，包含這個 HTML 元件，原因是 React 和 Vue 一樣都是一個 SPA (Single-Page-Application) 框架，透過單頁的 index.html 抽換其中的內容來滿足使用者的頁面操作，這行指令就是以此作為注入點將我們接下來要開發的內容注入到 id="root" 這個 HTML 元件中。

```html
<!-- index.html -->
<div id="root"></div>
```

而這個被注入的 `<APP />` 又是什麼呢 ? 看起來很像一般的 HTML 但又不太一樣，其實這就是 React 所開發的 JSX 語法 ; 而這個 App.js，其實就是這個專案的第一個 Component，透過一個 function 回傳 HTML，將所有頁面以非原生的 JavaScript 來完成

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)