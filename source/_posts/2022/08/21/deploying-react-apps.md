---
title: Deploying React Apps
abbrlink: 61941
date: 2022-08-21 11:36:00
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

在過去的課程中，我們都是透過 Node.js 啟動一個 local server，對專案進行即時的開發，實際上大部的開發時間也都是如此，但最後我們終究是是要將開發的專案部屬到真正的 server 以供其他使用者使用，接下來將探討 React Apps 從開發到部屬得流程

<!-- more -->

## Deploying Steps

在功能開發完成後，會進行以下步驟以完成正式上線:

- Test Code: 對功能進行測試，不管是人為測試又或是撰寫的測試，確保功能正盛運作

- Optimize Code: 程式碼的優化，其中包含 lazy loading 提高使用者體驗與網站效能

- Build App for Production: 不管使用的開發工具或框架是什麼，瀏覽器終究只看得懂 HTML、CSS & JavaScript，所以需要將程式碼打包成這些格式

- Upload Production Code to Server: 上傳這些打包後的程式碼到實際的 server

- Configure Server: 符合需求的 server 相關設定

## Adding Lazying Loading

React.lazy(() => import(`${path}`))

lazying loading 所要優化的重點就是，當使用者開啟網頁時，只載入頁面必要的程式碼，藉此提高網站效能，因為如果沒有特別作這方面的優化，通常是取得所有的程式碼後，再根據 route 決定要顯示的頁面需要取用哪一段程式碼

一段基本的 Rotuer 寫法如下

```js
import { Route, Switch, Redirect } from "react-router-dom";

import AllQuotes from "./pages/AllQuotes";
import NewQuotes from "./pages/NewQuotes";
import QuotesDetail from "./pages/QuotesDetail";
import NotFound from "./pages/NotFound";
import Layout from "./components/layout/Layout";
import LoadingSpinner from "./components/UI/LoadingSpinner";

const App = () => {
  return (
    <Layout>
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
          <NotFound />
        </Route>
      </Switch>
    </Layout>
  );
}

export default App;
```

### React.lazy()

而在 React Router 的專案中要加入 lazy loading 也非常容易，React 提供了一個 function `lazy()`，其中的參數為 callback function，回傳透過 JavaScript `import` 取得路徑

```js
// before lazy loading
import AllQuotes from './pages/AllQuotes';

// lazy loading
const AllQuotes = React.lazy(() => import('./pages/AllQuotes'));
```

### Suspense Component

但如果只做到這一步，會發現在切換 route 後畫面沒有顯示，開啟 devtools 會看到以下 error log

![](without-Suspense.png)

原因是加入 lazying loading 後，畫面所需的程式碼都是在切換 route 後才取得，所以需要有暫時的畫面來填補這段取得資料的空窗期，React 提供了 `Suspense` component，來解決這個問題，其中的 `fallback` attribute 可議值大入暫定頁面的 JSX，就會在這段取得資料的空窗期渲染出來

```js
import { Suspense } from 'react';
const App = () => {
  return (
    <Layout>
      <Suspense
        fallback={
          <div className="centered">
            <LoadingSpinner />
          </div>
        }
      >
      <Switch>
        <Route path="/" exact>
          <Redirect to="/quotes" />
        </Route>
        ...some routes else
      </Switch>
      </Suspense>
    </Layout>
  );
};
```  

加入 `Suspense` 後，頁面就可以正常呈現了，打開 devtools Network 會發現，每當切換 route 取得當下所需的程式碼時，會取得一個 chunk.js 的檔案，這就是加入 lazy loading 後，我們的程式碼會被分割成許多區塊，只在必要時載入

![](chunk.png)

## Building The Code For Production

不論是原生專案、Vue 或是其他框架，有透過 npm 管理過專案的開發者就不會對 package.json scripts 太陌生，create-react-app 同樣基於這樣的環境基礎來建立，開啟 package.json 就會有幾組預設指令，如下:

```json
{
  ...
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  ...
}
```

`start` 指令已經在每次啟動 local server 時反覆執行過了，`build` 就是將 React project打包成 HTML、CSS & JavaScript 所需的指令

```bash
$ npm build
```

執行後，會發現專案中出現了 build 資料夾，打開後會有許多原生檔案，這些就是 React project 打包後的產物，也是最後要放到實際 server 的檔案;另外，在打包後，就不要再對這些檔案進行修改了，因為每次執行打包指令後，這些檔案都會被再次編譯並覆蓋，所以如果有要調整的部分只能回到原有檔案些改後再次編譯

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[React Router](https://reactrouter.com/docs/en/v6/upgrading/v5)
