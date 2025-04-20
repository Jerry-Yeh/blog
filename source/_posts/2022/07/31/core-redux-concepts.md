---
title: Core Redux Concepts
abbrlink: 30803
date: 2022-07-31 10:19:30
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
  - Redux
categories: React
thumbnail:
banner:
description:
---

<!-- @format -->

Redux 基礎應用流程

<!-- more -->

在透過一個最單純的範例來了解 Redux 應用流程時，甚至不需要 create-react-app 來建立專案，只需要單純透過 Node.js 讓 JavaScript 可以在非瀏覽器環境中執行就可以了;首先，開啟一個新的專案資料夾，再透過以下指令安裝 Node.js 與 Redux

```bash
npm init -y

npm install redux
```

安裝後，因為是在 Node.js 環境，所以 import library 的方式有些微的差異:

```js
const redux = require("redux");
```

### crateStore()

接著可以透過 `redux.createStore()` 來建立 store，其中第一個參數就是 reducer callback function，在這裡只需要帶入，不需要即時呼叫它

```js
const store = redux.createStore(counterReducer);
```

### reducer callback function

而 reducer function 只是一個 general JavaScript function，其中兩個參數分別為:

- Old State: 舊的 state 值
- Dispatched Action: 修改 state 值的指令 Action

最後會回傳最新的 State，State 為 object

```js
const counterReducer = (state, action) => {
  return {
    counter: state.counter + 1;
  };
};
```

### store.getState()

如果要取得 state 的值，可以透過 `state.getState()` 取得，但如果直接執行 `node ${js file name}` 會出現以下 error log

```
TypeError: Cannot read properties of undefined (reading 'counter')
```

原因是沒有給定 state 初始值，最簡地的做法就是在 reducer function 中直接給定初始值，只有第一次會使用到，之後的 state 就會依照 return object 的值更新

```js
const counterReducer = (state = {counter: 0}, action) => {
  return {
    counter: state.counter + 1;
  };
};
```

### Subscribe

而一班的使用情境其實是會透過 component subscribe store 來取得 store 的值，作法如下:

```js
const counterSubscriber = () => {
  const latestState = store.getState();
  console.log(latestState);
};

store.subscribe(counterSubscriber);
```

結果會發現，並沒有顯示 subscriber 中的預期會顯示的值，原因是 subscribe 需要透過 dispatch 來觸發

### store.dispatch()

當我們透過 `store.dispatch()` 觸發 state 更新時，可以在其中帶入辨別行為的參數

```js
store.dispatch({ type: "increment" });
```

最終結果會得到 counterReducer 執行"兩次"的 `counter: 2`，原因是在 store 建立時，就先呼叫一次 reducer，如果執行 dispatch 就會再次被呼叫

## Look Into Action

當然有了 aciton 所指定的行為後，就可以在 reducer function 中藉由判斷 action 的指示對 state 做出對應的操作

```js
const counterReducer = (state = { counter: 0 }, action) => {
  switch (action.type) {
    case "increment":
      return {
        counter: state.counter + 1,
      };
    case "decrement":
      return {
        counter: state.counter - 1,
      };
    default:
      return state;
  }
};

const store = redux.createStore(counterReducer);

const counterSubscriber = () => {
  const latestState = store.getState();
  console.log("latestState", latestState);
};

store.subscribe(counterSubscriber);

store.dispatch({ type: "increment" }); // { counter: 1 }
store.dispatch({ type: "decrement" }); // { counter: 0 }
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
