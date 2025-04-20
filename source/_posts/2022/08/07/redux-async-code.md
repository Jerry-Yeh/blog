---
title: Redux & Async Code
abbrlink: 51228
date: 2022-08-07 12:15:19
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
  - Redux
categories: React
thumbnail:
banner:
description:
---

在 Redux 中，無法直接處理 async function，這可能會導致某些無法預期的資料遺漏，但資料的來源與更新往往是來自於後端 server，所以必須透過 async http request 取得;接下來，將介紹兩種如何在使用 Redux 時，處理 async function 的方式

<!-- more -->

## [Using useEffect() with Redux](https://github.com/Jerry-Yeh/-React-The-Complete-Guide-Section19/commit/639e0f8fd06fc0d1f1dd843ff5e84c75c1e5ac4e)

因為 Redux 無法接受 async function，因此如果要在更新 Redux 資料時同步更新 Server 資料，就可以透過 `useEffect()` hook 來監聽在 Redux 資料變更時，呼叫 API 更新 Server 資料，在這裡的範例中，Server 只是簡易的使用 firebase Database 示範，因此可以直接使用 PUT method 來取代其中的資料

```js
import { useEffect } from "react";
import { useSelector } from "react-redux";

const App = () => {
  const cart = useSelector((state) => state.cart.cartItems);

  useEffect(() => {
    fetch("https://react-http-77951-default-rtdb.firebaseio.com/cart.json", {
      method: "PUT",
      body: JSON.stringify(cart),
    });
  }, [cart]);

  return (...);
};
```

## Using an Action Creator Thunk

Redux Slice 本身無法處理 async function，但如果希望與 store 資料相關的 async 邏輯可以一同放置在 slice 檔案中管理，就可以使用到 Action Creator 這個方法

Action Creator 是位在 Slice 檔案內的 function，比較特別的是，其本身並非 async function，而是回傳 async function，async function 的第一個參數就是 dispatch。

```js
// App.js
import { Fragment, useEffect } from "react";
import { useSelector, useDispatch } from "react-redux";

import { sendCartData } from "./store/cart-slice";

const App = () => {
  const dispatch = useDispatch();
  const cart = useSelector(state => state.cart.cartItems);

  useEffect(() => {
    dispatch(sendCartData(cart));
  }, [cart, dispatch]);
};

// cart-slice.js
export const cartData = (cart) => {
  return async (dispatch) => {
    const sendRequest = async () => {
      const response = await fetch('url');
    };

    if (!response.ok) {
      throw new Error('Sending cart data failed!');
    }

    await sendRequet()
      .then(() => {});
      .catch(() => {});
  };
};
```

## 資料參考 

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)