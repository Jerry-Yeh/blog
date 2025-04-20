---
title: Handling Form Submission
abbrlink: 55451
date: 2022-06-07 23:25:09
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

<!-- @format -->

接下來將取得表單資料，並模擬將表單資料送出的動作

<!-- more -->

在原生 browser 中，如果是位於 form 表單內 `type="submit"` 的 button，在點擊時都會觸發 form 上預設的 event submit，在 React 中，按照慣例會在前方加上 on (onSubmit)

```js
// ExpenseForm.js
function ExpenseForm() {
  const submitHandler = (event) => {};

  return (
    <form onSubmit={submitHandler}>
      ...
      <button type="submit">Add Expense</button>
    </form>
  );
}
```

但原生的 sumbit event 會因為送出 request 而 reload browser，這不是我們期待的結果，所以需要透過一個 default event function `event.preventDefault` 來預防這件事發生

```js
// ExpenseForm.js
function ExpenseForm() {
  const submitHandler = (event) => {
    event.preventDefault();
  };

  return (
    <form onSubmit={submitHandler}>
      ...
      <button type="submit">Add Expense</button>
    </form>
  );
}
```

接著我們暫時先將三筆表單資料特自透過 `useState` 來建立，並在 submit event 中組合成一個 object 送出

```js
// ExpenseForm.js
function ExpenseForm() {
  const [enteredTitle, setEnteredTitle] = useState("");
  const [enteredAmount, setEnteredAmount] = useState("");
  const [enteredDate, setEnteredDate] = useState("");

  const submitHandler = (event) => {
    event.preventDefault();

  const expenseData = {
    title: enteredTitle,
    amount: enteredAmount,
    date: new Date(enteredDate),
  };

  console.log(expenseData);
  };

  return (
    <form onSubmit={submitHandler}>
      ...
      <button type="submit">Add Expense</button>
    </form>
  );
}
```

## Adding Two-Way Binding

到目前為止，我們都只是單方面的監聽使用者輸入的內容，但如果我們希望在點擊送出表單的按鈕 (Submit) 時，將所有欄位的資料清空，就需要用到 React 中一個很重要的觀念 - 雙向綁定 Two-Way Binding，以現有的範例延伸，我們只需要在 `input` 上加上屬性 `value`，並將 State 各自綁定上去，這一來就可以藉由改變 State 來改變 `input` 的內容了

```js
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

  const submitHandler = (event) => {
    event.preventDefault();

    setEnteredTitle('');
    setEnteredAmount('');
    setEnteredDate('');
  };

  return (
    <form onSubmit={submitHandler}>
      <div className="new-expense__controls">
        <div className="new-expense__control">
          <label>Title</label>
          <input
            type="text"
            value={enteredTitle}
            onChange={titleChangeHandler}
          />
        </div>
        <div className="new-expense__control">
          <label>Amount</label>
          <input
            type="number"
            min="0.01"
            step="0.01"
            value={enteredAmount}
            onChange={amountChangeHandler}
          />
        </div>
        <div className="new-expense__control">
          <label>Date</label>
          <input
            type="date"
            min="2019-01-01"
            max="2022-12-31"
            value={enteredDate}
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

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[Github](https://github.com/Jerry-Yeh/react-complete-guide/commit/48949796ffcf7c04fa32de9e096e8d39a4b71af9)
