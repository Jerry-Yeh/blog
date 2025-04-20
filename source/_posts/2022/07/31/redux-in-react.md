---
title: Redux in React
abbrlink: 39070
date: 2022-07-31 12:19:04
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
  - Redux
categories: React
thumbnail:
banner:
description:
---

<!-- @format -->

Redux 其實並不限於 React 專案之中，它可以被使用在任何 JavaScript 專案，接下來將介紹如何在 React 專案之中使用 Redux

<!-- more -->

## Install

其實 Redux 並不在乎 React，它是一個可以被單獨使用於 JavaScript 專案中的 library，而為了與 React 專案有更好的整合性，React 提供了另一個 library - react-redux 使得兩者在開發上有更好的兼融性

```bash
npm i redux react-redux
```

## Create Store

在 React 專案中，習慣透過建立一個名為 store 的資料夾管理 Redux 資料;首先，可以在 store 中建立一個 index.js 檔案，並在其中建立最基本的 Redux store，最後將 store export 供其他 component 使用

```js
// store/index.js
import { createStore } from "redux";

const counterReducer = (state = { counter: 0 }, action) => {
  if (action.type === "increment") {
    return {
      counter: state.counter + 1,
    };
  }

  if (action.type === "decrement") {
    return {
      counter: state.counter - 1,
    };
  }

  return state;
};

const store = createStore(counterReducer);

export default store;
```

## Providing the Store

在過去使用 context hook 統一管理資料時，會在根目錄進入點 index.js 中，透過 provider 包覆根元件 App，使其內部所有子元件都可以使用到 context 中的資料

同樣的，Redux 的使用方式也很類似，唯一比較特別的是，Provider 來自先前所額外安裝的 react-redux，在透過其 store 指定我們所建立的 store

```js
// index.js
import React from "react";
import ReactDOM from "react-dom/client";
import { Provider } from "react-redux";

import "./index.css";
import App from "./App";
import store from "./store/index";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

## [Using Redux Data in React Components](https://github.com/Jerry-Yeh/React-The-Complete-Guide-Section18/commit/a883be5cf7de5b83b66425b19c881380d3502f5f)

會放在 Redux 中的資料其目的不過是可以同事被多個 component 所使用，在 React functional component 專案中，可以透過 react-redux 中的 `useSelector()` hook 來取得 store state，其中的 callback function 第一個參數就是 state，可以直接指定 state 中的任一參數作為回傳的資料

```js
// Counter.js
import { useSelector } from "react-redux";

const Counter = () => {
  const counter = useSelector((state) => state.counter);
};
```

## [Dispatching Actions From Inside Components](https://github.com/Jerry-Yeh/React-The-Complete-Guide-Section18/commit/4d02f501a62b78e445a412a0457e6e5da527866d)

如果要對調整 store state 的值，就必須透過 dispatch 來操作，dispatch 中的參數代表預期的行為，行為所對應的操作邏輯則是會寫在 store dispatch function 之中

```js
import { useDispatch } from "react-redux";

const Counter = () => {
  const dispatch = useDispatch();

  const incrementHandler = () => {
    dispatch({ type: "increment" });
  };

  const decrementHandler = () => {
    dispatch({ type: "decrement" });
  };

  return (
    <div>
      <button onClick={incrementHandler}>Increment</button>
      <button onClick={decrementHandler}>Decrement</button>
    </div>
  );
};

export default Counter;
```

## [Redux with Classed-based Components](https://github.com/Jerry-Yeh/React-The-Complete-Guide-Section18/commit/15a15ff143c13409f9907365a818c283e86c5db0)

當然，Redux 也可以應用在 classed-based component 當中，只是使用方法有些微的不同

首先，透過 react-redux 取得 `connect` function，比較特別的是，class component 並非直接作為 `connect` 的參數呼叫，而是作為 `connect` 呼叫所回傳函式的參數;同樣地，class 中的 function 在 JSX 中呼叫時，需要透過 `bind(this)` 將 this 指向 class 本身

```js
import {Component} from 'react';
import {connect} from 'react-redux';

class Counter extends Component {
  incrementHandler() {}

  decrementHandler() {}

  render() {
    return (
      <div>
        <button onClick={this.incrementHandler.bind(this)}>Increment</button>
        <button onClick={this.decrementHandler.bind(this)}>Decrement</button>
      </div>
    );
  }
}

export default connect(){Counter};
```

接著，`connect()` function 中有兩個參數，都是 function，當然名稱可以自行定義，分別為以下兩者:

- mapStateToProps: 參數為 store state，回傳 component 所需 state
- mapActionToProps: 參數為 dispatch，回傳所需 dispatch function

兩者所回傳的值，可以直接成為 class props

```js
import {Component} from 'react';
import {connect} from 'react-redux';

class Counter extends Component {
  incrementHandler() {
    this.props.incrementHandler();
  }

  decrementHandler() {
    this.props.decrementHandler();
  }

  render() {
    return (
      ...
      <div className={classes.value}>{this.props.counter}</div>
      <div>
        <button onClick={this.incrementHandler.bind(this)}>Increment</button>
        <button onClick={this.decrementHandler.bind(this)}>Decrement</button>
      </div>
    );
  }
}

const mapStateToProps = state => {
  return {
    counter: state.counter,
  };
};

const mapActionToProps = dispatch => {
  return {
    incrementHandler: () => dispatch({type: 'increment'}),
    decrementHandler: () => dispatch({type: 'decrement'}),
  };
};

export default connect(mapStateToProps, mapActionToProps){Counter};
```

## [Attaching Payloads to Actions](https://github.com/Jerry-Yeh/React-The-Complete-Guide-Section18/commit/7fd7abe95693cb1762eab8ae4250d49e122e9a3a)

如果突然多了一個需求是要新增一個點一下可以加 5 的按鈕，直接的做法是在 reducer 中直接新增一個加 5 的情境

```js
// store/index.js
const counterReducer = (state: { counter: 0 }, action) => {
  switch (action.type) {
    case "increment":
      return {
        counter: state.counter + 1,
      };
    case "increaseBy5":
      return {
        counter: state.counter + 5,
      };
    case "decrement":
      return {
        counter: state.counter - 1,
      };
    default:
      return state;
  }
};
```

但這麼做有一個很大的缺點是，5 這個變數因為 hard code 缺乏彈性，如果往後有其他數字的需求，勢必就是不斷複製相同程式碼，所以比較好的做法就是讓 5 成為一個可彈性帶入的變數

```js
// store/index.js
const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    ...
    case "increase":
      return {
        counter: state.counter + action.amount,
      };
    ...
    default:
      return state;
  }
};

// Counter
const Counter = () => {
  const increase = () => {
    dispatch({type: 'increase', amount: 5});
  };

  return (
    <button onClick={increaseHandler}>Increase by 5</button>
  );
};
```

## [How To Work With Redux State Correctly](https://github.com/Jerry-Yeh/React-The-Complete-Guide-Section18/commit/a171cb396f8a45f835611f7a4f813abea3c41910)

在過去的範例中會發現，所有 reducer 在編輯 state 的值後，都會回傳一個全新的 object 來覆蓋原有的 state，而不是單純調整 state 的值

```js
// correct
const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    ...
    case "increase":
      return {
        counter: state.counter + action.amount,
      };
    ...
    default:
      return state;
  }
};

// error
const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    ...
    case "increase":
      state.counter += action.amount;
      return state;
    ...
    default:
      return state;
  }
};
```

因為在 Redux 中，State 的值是不能直接被 mutate 的，即使在基礎的應用上看似功能一切正常，也可能發生無法預期的 bug，如果有多項 state，但 action 只打算改變一項 state，可以透過 spread 的方式沿用其他不變的 state，再覆蓋欲更改的 State

```js
const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    ...
    case "increase":
      return {
        ...state,
        counter: state.counter + action.amount
      };
    ...
    default:
      return state;
  }
};
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
