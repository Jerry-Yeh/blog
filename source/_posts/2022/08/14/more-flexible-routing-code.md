---
title: More flexible routing code
abbrlink: 33177
date: 2022-08-14 16:05:35
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
  - React Router
categories: React
thumbnail:
banner:
description:
---

在某些需求下，除了一層 route 切換主頁面之外，可能還會再有內層 route 切換內部元件，形成 Nested Route，但為了避免改動上層路徑名稱就要接連改動下層的名稱，React Router 提供了更彈性的方式取得當前路徑參數，避免 hard code 的情況發生

<!-- more -->

## [useRouteMatch()](https://github.com/Jerry-Yeh/-React-The-Complete-Guide-Section20-2/commit/3baec000822ab97ea9955aed0b572cf723dcd0a9)

React Router 提供了另一了 hook - `useRouteMatch()`，這個 hook 所回傳的 object 包含以下 key-value pair

- isExact: 是否為 `exact` 路徑完全符合的條件
- params: 以 object 列出所有 param
- path: 可直接提供 `Route path` 的路徑，ex: /route/:id
- url: 將 path param 轉成取得的值，可直接提供 `Link to` 路徑，ex: /route/q1

```js
import { Fragment } from 'react';
import { useRouteMatch } from 'react-router-dom';

const someChildRoute = () => {
  const match = useRouteMatch();

  return (
    <Fragment>
      <Route path={match.path} exact>
        <Link to={`${match.url}/comments`}>
          Load Comments
        </Link>
      </Route>
      <Route path={`${match.path}/comments`}>
        <Comments />
      </Route>
    </Fragment>
  );
};
```

## useRouteMatch() vs useLocation()

相較於 `useRouteMatch()`，`useLocation()` 提供了當下的完整路徑和 query params，適用於需要完整路徑或附帶參數的時候;`useRouteMatch()` 則較常用於 Nested Route，因為回傳的是最接近的匹配路徑或其 Parent Route

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[React Router](https://reactrouter.com/)
