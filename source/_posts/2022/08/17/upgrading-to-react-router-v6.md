---
title: Upgrading to React Router v6
abbrlink: 188
date: 2022-08-17 22:28:00
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
  - React Router
categories: React
thumbnail:
banner:
description:
---

在先前的課程中，都是以 React Router v5 作為範例，但在這篇筆記紀錄的當下，React Router 已經推出 v6 了，所以接下來將升級 v5 的專案至 v6，並說明其所帶來的好處

<!-- more -->

## Install

不論是否已經安裝 v5 的專案，要升級到 v6 同樣非常簡單，只要執行下列指令即可

```bash
$ npm i react-router-dom@6
```

## v5 Switch => v6 Routes

在 v5 中，會透過 `Switch` Component 包覆所有 `Route` 以解決渲染出其中所有符合條件的 `Route`，v6 中則改以 `Routes` 取代

```js
const App = () => {
  return (
    // v5
    <div>
      <Switch>
        <Route>
          ...
        <Route/>
        <Route>
          ...
        <Route/>
      </Switch>
    </div>

    // v6
    <div>
      <Routes>
        <Route>
          ...
        <Route/>
        <Route>
          ...
        <Route/>
      </Routes>
    </div>
  );
};
```

## Route child component -> element={JSX component}

在 v5 `Route` 中，會將欲在這個路徑下顯示的 Component 作為 child component，v6 則改以 `element` attribute 取代，但這裡要特別注意的是，不是單純放入 component，而是放入 component JSX

```js
// v5
<Route to="/welcome">
  <Welcome />
</Route>

// v6
<Route to="/welcome" element={<Welcome />}>
```

## remove exact

`exact` attribute 則被移除了，取而代之的是，v6 中有更好的演算法來判斷最佳路徑，所以如果有以下路徑，在進到 `/products/p1` 時，也不會渲染 `/products`

```js
<Route path="/products" element={<Products />} />
<Route path="/products/:productId" element={<ProductDetail />} />
```

但如果希望同時被渲染，可以在後方加上 `/*`，就會出現和 v5 不加 `exact` 同樣的結果

```js
<Route path="/products/*" element={<Products />} />
<Route path="/products/:productId" element={<ProductDetail />} />
```

另外，v6 中更好的演算法也讓我們不用擔心 `Route` 的放置順序，如果出現下列寫法，並選擇到 `/products/edit`，也不會因為由上而下的緣故選擇 `/products/:productId`，而是選擇最符合結果的選項

```js
<Route path="/products/:productId" element={<ProductDetail />} />
<Route path="/products/edit" element={<ProductEdit />} />
```

## Link & NavLink

在 `Link` component 的使用上並沒有差異，但在 NavLink 的使用上有些許的差異是，當我們使用客製 class 時，在 v5 使用的是 `activeClassName`，但在 v6 被移除了，取而代之的是使用一般的 `className`，但其中的值為 function，function 第一個參數為 React Router 所提供的 `navData`(名稱自訂)，其中的 `isActive` 如果在路徑有被正常指向就會是 `true`，所以寫法差異如下

```js
// v5
<NavLink activeClassName={classes.active} to="/welcome">
  Welcome
</NavLink>

// v6
<NavLink className={(navData) => nvaData.isActive ? classes.active : ''} to="/welcome">
  Welcome
</NavLink>
```

## Redirect -> Navigate

v5 中，可以透過 `Redirect` 將網址重新導向期待的頁面，在 v6 中則由 `Navigate` 所取代，但仍然需要改寫到 element attribute 之中，完整的寫法可以再補上 `replace` attribute

```js
// v5
<Route path="/">
  <Redirect to="/welcome" />
</Route>

// v6
<Route path="/" element={<Navigate replace to="/welcome" />}/>
```

## Nested Route

在 v5 Nested Route 的寫法就是直接在 Component 中加上一組 `Route`，但在 v6 中，`Route` 無法在沒有 `Routes` 的情況下單獨出現

```js
// v5
<Route path="/welcome/new-user">
  <p>Welcome, new user!</p>
</Route>

// v6
<Routes>
  <Route path="/welcome/new-user" element={<p>Welcome, new user!</p>} />
</Routes>
```

另外，在 v6 中，外層 `Route` 如果還有內層 `Route`，就需要在 url 後方加上 `/*`，好處是內層的 url 就不用再重複外層的 url，只要寫上附加的 url 即可

```js
const App = () => {
  return (
    <Routes>
      <Route path="/welcome/*">
    </Routes>
  );
};

// v5
const Welcome = () => {
  return (
    <Link to="/welcome/new-user">
    <Route path="/welcome/new-user">
      <p>Welcome, new user!</p>
    </Route>
  );
};

// v5
const Welcome = () => {
  return (
    <Link to="/new-user">
    <Routes>
      <Route path="/new-user" element={<p>Welcome, new user!</p>} />
    </Routes>
  );
};
```

### 改寫

v6 還提供了另一種 Nested Route 的寫法，可以將所有 Nested Route 寫在一起

```js
const App = () => {
  return (
    <Routes>
      <Route path="/welcome/*" element={<Welcome />}>
        <Route path="new-user" element={<p>Welcome, new user!</p>} />
      </Route>
    </Routes>
  );
};
```

但這裡還有一個問題是，Child Route element 不知道應該要放在哪裡，所以 React Router 提供了另一個 component - `Outlet` 來指定放置的位置

```js
const Welcome = () => {
  return (
    <section>
      <h1>The Welcome Page</h1>
      <Link to="new-user">New User</Link>
      <Outlet />
    </section>
  );
};
```

這兩種寫法完全由個人喜好自行決定

## useHistory() -> useNavigate()

在 v5 中，如果是依賴程式在滿足某種條件下自動切換 route，可以透過 `useHistory()` hook 來改變 route，v6 中則由 `useNavigate()` 取代，而且不需要 `push` function，可以直接於回傳的 function 所執行

```js
// v5
const history = useHistory();
history.push('/welcome');

// v6
const navigate = useNavigate();
navigate('welcome');
```

## Prompt -> ❌

在 v5 中，`Prompt` component 提供了一層確認 alert 作為使用者操作行為的防呆機制，但在 v6 中被移除了，只能自行撰寫 component 來維持這個機制，在未來可能會被重新加回來也說不定

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[React Router](https://reactrouter.com/docs/en/v6/upgrading/v5)
