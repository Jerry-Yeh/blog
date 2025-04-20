---
title: React Router
abbrlink: 36407
date: 2022-08-13 10:23:55
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
  - React Router
categories: React
thumbnail:
banner:
description:
---

<!-- @format -->

React 是一個 single-page application，顧名思義就是不論如何切換頁面，都是在單一個 html 檔案內替換其中的內容，而非傳統網頁在切換頁面時，都會透過對 server 發出 request 取得新的 html 檔案再渲染到 browser 上，這麼做得好的不外乎就是為了避免頻繁的在切換頁面時送出 request 以提高網站效能，在 React 社群中，有一個很好用的 3rd-part library - React Rotuer 替我們解決了這個問題。

<!-- more -->

## What is Routing & Why ?

在 React 這種 client-side render 的 single-page application 中，即便如何切換頁面，其實都是在同一個 html 檔案內並共用同一個 url，只是再透過 url 上的後綴以辨別當下頁面，傳統 server-side render 的網站的每一個頁面都是一個 html 檔案，所以當使用者切換頁面時，都需要送出 reqeust 向 server 取得 html 再渲染於網頁上，這麼做有一個很明顯的問題就是頻繁的送出 reqeust 可能會造成效能上的消耗已影響使用者體驗。

而 client-side render 則是透過 JavaScript 在 browser 中處理頁面切換的邏輯，向後端發出的 request 則改為單純的資料交換與操作，因此只會有一個 html 檔案與一個實際上的 url，只是透過 client routing 加上後綴以辨別頁面。

## [Installing React Router](https://www.npmjs.com/package/react-router)

在安裝 React Router 時，比較特別的是，並非直接安裝 react-router，而是安裝 react-router-dom or react-router-native，雖然這篇筆記紀錄的當下已經有了 v6，但接下來的範例會先以 v5 來操作，最後會再分享升級到 v6 的方式

```bash
$ npm i react-router-dom@5
```

## Defining & Using Routes

透過 Route 來控制 component 顯示的情況就會是每個 url 會有對應的 component，如下:

```js
// our-domain.com/welcome => Welcome Component
// our-domain.com/products => Products Component
```

首先，需要在根元件 App.js 中，註冊每個 component 所在的 route;這裡需要特別注意的是，所有 React Router function 的使用都是來自先前所安裝的 react-router-dom 而不是 react-router。

import `Route` JSX 後，其中的 `path` attribute 就是自定義的 route 名稱，內部則放置當切換到這個 route 時，預期顯示的 component

```js
// App.js

import { Route } from "react-router-dom";

import Products from "./components/pages/Products";
import Welcome from "./components/pages/Welcome";

const App = () => {
  return (
    <div>
      <Route path="/welcome">
        <Welcome />
      </Route>
      <Route path="/products">
        <Products />
      </Route>
    </div>
  );
};
```

在完成 route & component 的對應關係後，還需要到 index.js 中完成最後一個動作上述的註冊才會生效，那就是 import `BrowserRotuer` JSX 包覆 App

```js
// index.js

import ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router-dom";

import "./index.css";
import App from "./App";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

## [Working with Links](https://reactrouter.com/docs/en/v6/components/link)

a tag href will leave one single-page and create a new page, it will lose state in our page

有了 route & component 的定義後，接下來當然就是要嘗試切換 route 來顯示不同的 component，使用原生 html 的 `a` element 就可以切換頁面的目的

```js
// MainHeader.js
const MainHeader = () => {
  return (
    <header>
      <ul>
        <li>
          <a href="/welcome">Welcome</a>
        </li>
        <li>
          <a href="/products">Products</a>
        </li>
      </ul>
    </header>
  );
};

export default MainHeader;

// App.js
const App = () => {
  return (
    <div>
      <MainHeader />
      <Route path="/welcome">
        <Welcome />
      </Route>
      <Route path="/products">
        <Products />
      </Route>
    </div>
  );
};
```

點擊這兩個 `a` link 後，確實會如我們所預期的切換頁面，但這裡有一個很大的問題是，當我們點擊 `a` link 後，他預設的行為其實是送出 reqeust 取得新的 html 渲染於網頁上，這明顯違背了 single-page 的設計初衷，進而導致 state 資料的遺失，所以畫面才會閃了一下，這裡可以進一步打開 devtools Network 來確認聳出了 reqeust

為了解決這個問題，React Router 提供了另一個 JSX `Link` 這個問題，只要透過 `Link` 來取代 `a` element 切換 route，再透過 `to` 指定目標 route，就可以達到在 single-page 內切換 route 的目的了

```js
import { Link } from "react-router-dom";

const MainHeader = () => {
  return (
    <header>
      <ul>
        <li>
          <Link to="/welcome">Welcome</Link>
        </li>
        <li>
          <Link to="/products">Products</Link>
        </li>
      </ul>
    </header>
  );
};

export default MainHeader;
```

## [Using NavLinks](https://reactrouter.com/docs/en/v6/components/nav-link)

許多 UI 為了讓使用者更明確知道當下所在頁面，會對該切換頁籤加上特別的樣式以做區分，React Router 就提供了另一個 JSX `NavLink` 來附加客製化的 CSS，只要將 `Link` 由 `NavLink` 取代，並透過 `activeClassName` 就可以加上 CSS

```css
/* MainHeader.module.css */
.header {
  ...;
}

.header a.active {
  ...;
}
```

```js
// MainHeader.js
import { NavLink } from "react-router-dom";

import classes from "./MainHeader.module.css";

const MainHeader = () => {
  return (
    <header className={classes.header}>
      <ul>
        <li>
          <NavLink activeClassName={classes.active} to="/welcome">
            Welcome
          </NavLink>
        </li>
        <li>
          <NavLink activeClassName={classes.active} to="/products">
            Products
          </NavLink>
        </li>
      </ul>
    </header>
  );
};

export default MainHeader;
```

## [A Not Found Page](https://github.com/Jerry-Yeh/-React-The-Complete-Guide-Section20-2/commit/7460b91ee6bf8b8fe3505f84c6fa18c45516f31d)

當使用者瀏覽網頁時，可能因為網址輸入錯誤或其他原因導致沒有對應的頁面已顯示，在 React Router 中，`Route` 中的 `path` 可以使用 `*` 代表所有其他的路徑，只要是沒有對應頁面的路徑預設都會被導到此頁面

```js
const App = () => {
  return (
    <Switch>
      <Route path="/" exact>
        <Redirect to="/quotes" />
      </Route>
      <Route path="/quotes" exact>
        <AllQuotes />
      </Route>
      <Route path="/quotes/:quoteId">
        <QuotesDetail />
      </Route>
      <Route path="/new-quote">
        <NewQuotes />
      </Route>
      <Route path="*">
        {" "}
        // 我代表所有其他頁面
        <NotFound />
      </Route>
    </Switch>
  );
};
```

## [Implementing Programmatic Mavigation](https://github.com/Jerry-Yeh/-React-The-Complete-Guide-Section20-2/commit/de68c0b5e2f2a4c485968396b3ef52ce1408af35)

在很多使用情境中，頁面的跳轉並非使用者動作執行的當下就發生，而是在觸發條件後，需要完成特定的程式邏輯，如送出資料，才會進行頁面的跳轉，所以頁面跳轉的行為就不直接附加在按鈕上

React Router 提供了另一個 hook - `useHistory()` 來解決這個問題，其所回傳的 object 中，有一個 `push` method 可以指定目標的路徑

```js
import { useHistory } from "react-router-dom";

import QuoteForm from "../components/quotes/QuoteForm";

const NewQuotes = () => {
  const history = useHistory();

  const addQuoteHandler = (quoteData) => {
    console.log("addQuoteHandler", quoteData); // 我是 http request

    history.push("/quotes"); // 送出資料後我才跳轉
  };

  return <QuoteForm onAddQuote={addQuoteHandler} />;
};
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[React Router](https://reactrouter.com/)
