---
title: Form Inputs
abbrlink: 31080
date: 2022-06-06 23:29:30
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

<!-- @format -->

接下來將會加入 Expense APP 最基本的功能 - 提供使用者自行新增 Expense Item

<!-- more -->

## Adding Form Inputs

首先，我們新增一組 Form Component 供使用者填入 Expense Item 資訊，如下:

```js
// ExpenseForm.js
import React from 'react';
import './ExpenseForm.css';

const ExpenseForm = () => {
  return (
    <form>
      <div className='new-expense__controls'>
        <div className='new-expense__control'>
          <label>Title</label>
          <input type='text' />
        </div>
        <div className='new-expense__control'>
          <label>Amount</label>
          <input type='number' min='0.01' step='0.01' />
        </div>
        <div className='new-expense__control'>
          <label>Date</label>
          <input type='date' min='2019-01-01' max='2022-12-31' />
        </div>
      </div>
      <div className='new-expense__actions'>
        <button type='submit'>Add Expense</button>
      </div>
    </form>
  );
};

export default ExpenseForm;

// NewExpense.js
import React from 'react';

import ExpenseForm from './ExpenseForm';
import './NewExpense.css';

const NewExpense = () => {
  return (
    <div className='new-expense'>
      <ExpenseForm />
    </div>
  );
};

export default NewExpense;
```

## Listening to User Input

接著我們需要監聽使用者在 Form 表單中所輸入的內容，如果是原生 JavaScript 的寫法就會是以下這樣

```js
doucment.getElementById("").addEventListener("click", (event) => {});
```

在 React 中，我們可以在 `input` 上透過 `onChange` 來監聽它的變化，同樣不需要直接呼叫監聽的 function

```js
// ExpenseForm.js
import React from "react";
import "./ExpenseForm.css";

const ExpenseForm = () => {
  const titleChangeHandler = () => {
    console.log("Title changed!");
  };
  return (
    <form>
      <div className="new-expense__controls">
        <div className="new-expense__control">
          <label>Title</label>
          <input type="text" onChange={titleChangeHandler} />
        </div>
        <div className="new-expense__control">
          <label>Amount</label>
          <input type="number" min="0.01" step="0.01" />
        </div>
        <div className="new-expense__control">
          <label>Date</label>
          <input type="date" min="2019-01-01" max="2022-12-31" />
        </div>
      </div>
      <div className="new-expense__actions">
        <button type="submit">Add Expense</button>
      </div>
    </form>
  );
};
```

但目前我們只監聽到 `input` 變動的行為，還未取得到它的值，這時候就可以透過 handler 的預設參數 `event` 取得，我們可以透過 `console.log` 來查看它究竟是什麼

```js
const titleChangeHandler = (event) => {
  console.log(event);
};
```

會發現包含了許多屬性，其中 `target` 直接指向了 DOM element，而最常使用的 `target.value` 就是我們要取得的值了

![](event.png)

## Working with Multiple States

接著我們還需要透過先前的 `useState` 來儲存每次 `input` 輸入所更新的值，原因是透過這樣的方式儲存，就能避免每次 Component Functon re-render 資料被刷新的問題了 ; 在這組表單中，有 3 格欄位，基本用法上我們可以分成三 3 組 `useState` 來個別儲存資料，如下:

```js
// ExpenseForm.js
/** @format */

import React, { useState } from "react";

import "./ExpenseForm.css";

const ExpenseForm = () => {
  const [enteredTitle, setEnteredTitle] = useState("");
  const [enteredAmount, setEnteredAmount] = useState("");
  const [enteredDate, setEnteredDate] = useState("");

  const titleChangeHandler = (event) => {
    setEnteredTitle(event.target.value);
  };

  const amountChangeHandler = (event) => {
    setEnteredAmount(event.target.value);
  };

  const dateChangeHandler = (event) => {
    setEnteredDate(event.target.value);
  };

  return (
    <form>
      <div className="new-expense__controls">
        <div className="new-expense__control">
          <label>Title</label>
          <input type="text" onChange={titleChangeHandler} />
        </div>
        <div className="new-expense__control">
          <label>Amount</label>
          <input
            type="number"
            min="0.01"
            step="0.01"
            onChange={amountChangeHandler}
          />
        </div>
        <div className="new-expense__control">
          <label>Date</label>
          <input
            type="date"
            min="2019-01-01"
            max="2022-12-31"
            onChange={dateChangeHandler}
          />
        </div>
      </div>
      <div className="new-expense__actions">
        <button type="submit">Add Expense</button>
      </div>
    </form>
  );
};

export default ExpenseForm;
```

## Using One State Instead (And What's Better)

以上的作法在運作上完全沒有問題，但因為三組類似的 input 欄位使我們寫了相同的三組 `useState` 來定義這些資料，我們也可以改以一個 object 一口氣囊括這三個資料欄位來改寫

```js
// Before
const [enteredTitle, setEnteredTitle] = useState("");
const [enteredAmount, setEnteredAmount] = useState("");
const [enteredDate, setEnteredDate] = useState("");

// After
const [userInput, setUserInput] = useState({
  enteredTitle: "",
  enteredAmount: "",
  enteredDate: "",
});
```

那麼問題來了，每次使用者在透過 input 輸入資料時，其實只會更新這個 object data 中的一個屬性，那我們應該如何只更新這個屬性並保留其他兩個屬性得值呢 ?

ES6 提供了一個很方便的方法 - Spread `...`，透過這個方式，我們可以將舊的值 (userInput) 展開，並在後方以新的值賦寫目標屬性，即可得到只更新一個值的 object 了

```js
const titleChangeHandler = (event) => {
  setUserInput({
    ...userInput,
    enteredTitle: event.target.value,
  });
};

const amountChangeHandler = (event) => {
  setUserInput({
    ...userInput,
    enteredAmount: event.target.value,
  });
};

const dateChangeHandler = (event) => {
  setUserInput({
    ...userInput,
    enteredDate: event.target.value,
  });
};
```

## Updating State That Depends On The Previous State

以上的寫法在大部分的情況都沒有問題，但有時候我們會同時更新很多 State，而 React 在收到更新指令 (set function) 後，其實不會馬上去做更新，這會導致我們基於舊資料 (prevState) 所做的更新沒有可能會沒有取得最新的資料 ; React 在這種情況下提供了一個更安全的方式來解決這個問題，那就是將 set function 內的值改成一個 callback function，而這個 callback function 第一個參數就是舊資料 (preState)，最後 React 會以 return 的值最後新的值來更新 State。

```js
// Before
const titleChangeHandler = (event) => {
  setUserInput({
    ...userInput,
    enteredTitle: event.target.value,
  });
};

// After
const titleChangeHandler = (event) => {
  setUserInput((prevState) => {
    return {
      ...prevState,
      enteredTitle: event.target.value,
    };
  });
};
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[Github](https://github.com/Jerry-Yeh/react-complete-guide/commit/b6954cdf79bc861c388dde6dac2184bef5608d89)
