---
title: Working with Multiple Slices
abbrlink: 64624
date: 2022-08-04 23:04:07
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
  - Redux
categories: React
thumbnail:
banner:
description:
---

在一般的專案開發中，功能當然不會只有一個 counter 這麼簡單，一個基本的網站可能就包含登入、列表等功能，而登入的狀態是很典型會被應用在許多不同 component 的資料 (app-wide state)，接下來就會探討如何在 Redux 中管理多項不同的功能

<!-- more -->

## [Multiple Slices](https://github.com/Jerry-Yeh/React-The-Complete-Guide-Section18/commit/fce4f16b393e719b8b81292e143f7dbf2f66b6b7)

延續上一章的 Counter 功能，接著要在專案中加入登入的功能，可能會有一組新的 component 如下:

```js
import { Fragment } from "react";

import Counter from "./components/Counter";
import Auth from "./components/Auth";
import Header from "./components/Header";

function App() {
  return (
    <Fragment>
      <Header />
      <Auth />
      <Counter />
    </Fragment>
  );
}

export default App;
```

接著在 Redux store 中加入登入的功能，以技術上來說，我們完全可以在已存在的 counterSlice 中加入登入相關的 state & action，但在開發時，為了更好的維護專案，我們盡可能還是將不同的功能加以區分，因此額外建立一個 authSlice 是更好的解決方式

```js
const counterSlice = createSlice({});

const initialAuthState = {
  isAuthenticated: false,
};

const authSlice = createSlice({
  name: 'auth',
  initialState: initialAuthState,
  reducers: {
    login(state) {
      state.isAuthenticated = true;
    },
    logout(state) {
      state.isAuthenticated = false;
    },
  }
})
```

但這裡要注意的是，無論我們因應功能需求增加了多少 slice,仍舊只會有一個 store，store 有只會有一個 reducer，因此如果出現多個 slice 的情況，就要將 reducer 改成 object，其中每一個 key-value 就代表一組 slice

```js
const store = configureStore({
  // reducer: counterSlice.reducer,
  reducer: { counter: counterSlice.reducer, auth: authSlice.reducer },
});
```

actions 則可以個別 export

```js
export const counterActions = counterSlice.actions;
export const authActions = authSlice.actions;
```

而在 component 的使用上有需要些許的調整，在使用 state 時，需要特別指明是哪一組 slice，名稱就來自 store reducer 中自定義的 key

```js
const Counter = () => {
  const dispatch = useDispatch();
  const counter = useSelector((state) => state.counter.counter);
  const show = useSelector((state) => state.counter.showCounter);
};
```

## [Splitting Our Code](https://github.com/Jerry-Yeh/React-The-Complete-Guide-Section18/commit/d493f258f093b93bbebf141bb705fedc0fe00e96)

隨著開發的功能越來越豐富，如果都將所有 Slice 寫在 store/index.js 內，這個檔案就會變得肥大難以維護，所以在開發時通常會將每個 Slice 以單一檔案個別管理，再統一匯入  store/index.js 建立 store

在建立 store 時，其實只需要 Slice.reducer，所以每個 Slice export 可以只提供 Slice.reducer;另外，Actions 則由每個 Slice 檔案個別 export

```js
// store/counter.js
import { createSlice } from "@reduxjs/toolkit";

const initialState = {
  counter: 0,
  showCounter: true,
};

const counterSlice = createSlice({
  name: "counter",
  initialState,
  reducers: {
    ...
  },
});

export const counterActions = counterSlice.actions;
export default counterSlice.reducer;

// store/index.js
import { configureStore } from "@reduxjs/toolkit";

import counterReducer from "./counter";
import authReducer from "./auth";

const store = configureStore({
  reducer: { counter: counterReducer, auth: authReducer },
});

export default store;
```

### Component Import

在 Component 的使用上，因為 Actions 改由個別 Slice export，所以當然也需要改從 Slice 檔案 import

```js
// components/Counter.js

import { useSelector, useDispatch } from 'react-redux';
import { counterActions } from '../store/counter';

const Counter = () => {
  ...
};
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)