---
title: Adding State Slices
abbrlink: 37510
date: 2022-07-31 17:33:13
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
  - Redux
categories: React
thumbnail:
banner:
description:
---

<!-- @format -->

接下來將介紹另一個 libray - Redux Toolkit 使我們在使用 Redux 時更加簡潔

<!-- more -->

## Install

由 [Redux Toolkit 官網](https://redux-toolkit.js.org/introduction/getting-started) 可以看到，直接透過 `npm install` 安裝了，而且因為 Redux Toolkit 預設已經安裝 Redux 了，所以如果已經安裝 Redux 就可已將其解除安裝

```bash
npm install @reduxjs/toolkit

npm uninstall redux
```

## [Adding State Slices](https://github.com/Jerry-Yeh/React-The-Complete-Guide-Section18/commit/3f67e11a9328825018f0d9b6c6af6fda169cb6ab)

首先，透過 `createSlice` 建立 store，其中第一個參數為 object，分別包含以下屬性

- name: 自定義名稱
- initialState: state 初始值，同樣為一個 object，各個 state 以其屬性定義
- reducers: 其值同樣為一個 object，action 所對應的 reducer function 可以直接被定義在其中

一段 Redux 基本寫法如下:

```js
import { createStore } from "redux";

const initialState = {
  counter: 0,
  showCounter: true,
};

const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    case "increment":
      return {
        ...state,
        counter: state.counter + 1,
      };
    case "increase":
      return {
        ...state,
        counter: state.counter + action.amount,
      };
    case "decrement":
      return {
        ...state,
        counter: state.counter - 1,
      };
    case "toggle":
      return {
        ...state,
        showCounter: !state.showCounter,
      };
    default:
      return state;
  }
};
```

接下來透過 Redux Toolkit 改寫:

```js
import { createSlice } from "@reduxjs/toolkit";

createSlice({
  name: "counter",
  initialState,
  reducers: {
    increment(state) {
      state.counter++;
    },
    decrement(state) {
      state.counter--;
    },
    increase(state, action) {
      state.counter += action.amount;
    },
    toogleCounter(state) {
      state.showCounter = !state.showCounter;
    },
  },
});
```

這裡可能會有個疑問是，在 reducer function 中不是有提到現有 state 不應該直接被更改，而是需要透過新的 state 來覆蓋原有的值嗎 ?

這裡可以直接修改的原因是當我們使用 Redux Toolkit 的時候，底層有一個名為 imgur 的工具，會自動偵測我們修改了哪一個 state，保留沒有動到的 state 並複製一組新的 object 來取代所有 state，所以其實更新 state 的方式還是一樣的

## [Connecting Redux Toolkit State](https://github.com/Jerry-Yeh/React-The-Complete-Guide-Section18/commit/17665f54e27892a8b261e728ede443f9f760397a)

透過 `createSlice()` 建立 reducer function 後，還需要讓它可以被取得，這時候就會需要另一個 function - `configureStore()`;首先，將剛才透過 `createSlice()` 所建立的 object 透過一個參數儲存

```js
import { createSlice, configureStore } from "@reduxjs/toolkit";

const counterSlice = createSlice({
  name: "counter",
  initialState,
  reducers: {
    increment(state) {
      state.counter++;
    },
    decrement(state) {
      state.counter--;
    },
    increase(state, action) {
      state.counter += action.amount;
    },
    toogleCounter(state) {
      state.showCounter = !state.showCounter;
    },
  },
});
```

接下來呼叫 `configureStore()`，其參數為一個 object，其中的 key reducer 可以直接將剛才的變數 `counterSlice.reducer` 作為其 value

```js
const store = configureStore({
  reducer: counterSlice.reduer,
});
```

問題來了，counter 可能只是一個專案裡的其中一個功能而已，如果有更多的 reducer，該如何定義 store 呢 ?

這裡只要將 `configureStore` 中的 reduer 改成一個 object，其中的 key 可以自行定義，如: counter，value 同樣為 `counterSlice.reducer`

```js
const store = configureStore({
  reducer: {
    counter: counterSlice.reducer,
  },
});
```

## [Migrating Everything to Redux Toolkit](https://github.com/Jerry-Yeh/React-The-Complete-Guide-Section18/commit/b651d24fc9ef14163db16f670c3d656cd844dc43)

有了這些定義後，接下來的問題就是如何讓其他 component 使用其中的資料了，當我們透過 `createSlice()` 建立 reducer function 時，可以透過其回傳的 object key - actions 取得其中所有 reducer action，所以只要將這些 actions export 出去便可以供其他 component 使用

```js
export const counterActions = counterSlice.actions;
```

而 component 要使用也很簡單，只要透過 import 取得 actions，就可以在 dispatch 中呼叫 action

```js
import { useDispatch } from "react-redux";
import { counterActions } from "../store/index";

const Counter = () => {
  const dispatch = useDispatch();

  const toggleCounterHandler = () => {
    // dispatch({ type: 'toggle' });
    dispatch(counterActions.toggleCounter());
  };
};
```

比較是如果需要帶入相關參數，可以直接帶入 primitive type 或 object，store 中的 action 則會以 `payload` 取得這個參數

```js
// Counter.js
const Counter = () => {
  const increaseHandler = () => {
    // dispatch({ type: 'increase', amount: 5 });
    dispatch(counterActions.increase(5)); // { type: SOME_UNIQUE_IDENTIFIER, payload: 5 }
  };
};

// index.js
const counterSlice = createSlice({
  name: "counter",
  initialState,
  reducers: {
    increase(state, action) {
      state.counter += action.payload;
    },
  },
});
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
