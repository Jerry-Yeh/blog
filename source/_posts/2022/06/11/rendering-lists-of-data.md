---
title: Rendering Lists of Data
abbrlink: 40578
date: 2022-06-11 17:03:38
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

<!-- @format -->

一般的使用者介面是透過資料動態呈現畫面的，延續這個範例，消費清單的比數應該根據資料的比數對應呈現，接下來畫面 (JSX) 將透過資料來產生

<!-- more -->

到目前為止，ExpenseItem 是透過 hard code 的方式固定 4 筆在頁面中，並沒有依照資料動態產生

```js
//Expenses.js

import React, { useState } from "react";

import ExpenseItem from "./ExpenseItem";
import "./Expenses.css";
import Card from "../UI/Card";
import ExpensesFilter from "./ExpenseFilter";

const Expense = (props) => {
  const [filteredYear, setFilteredYear] = useState("2020");
  const filterChangeHandler = (selectedYear) => {
    console.log("selectedYear", selectedYear);
  };
  return (
    <Card className="expenses">
      <ExpensesFilter
        selected={filteredYear}
        onChnageFilter={filterChangeHandler}
      />
      <ExpenseItem
        title={props.items[0].title}
        amount={props.items[0].amount}
        date={props.items[0].date}
      />
      <ExpenseItem
        title={props.items[1].title}
        amount={props.items[1].amount}
        date={props.items[1].date}
      />
      <ExpenseItem
        title={props.items[2].title}
        amount={props.items[2].amount}
        date={props.items[2].date}
      />
      <ExpenseItem
        title={props.items[3].title}
        amount={props.items[3].amount}
        date={props.items[3].date}
      />
    </Card>
  );
};

export default Expense;
```

而在 React 中，我們可以在 JSX 內透過透過大括號 {} 寫入任何 JS code，而如果寫入的是一個包含 JSX 的 Aarry，甚至可以直接被渲染出來

```js
function Expenses(props) {
  ...
  return (
    <div>
      ...
      {[<ExpenseItem />, <ExpenseItem />]}
      ...
    </div>
  );
}
```

透過這個功能，我們就可以動態的藉由傳進來的資料來產生對應的 JSX

```js
function Expenses(props) {
  ...
  return (
    <div>
      ...
      {props.items.map((expense) => (
        <ExpenseItem
          title={expense.title}
          amount={expense.amount}
          date={expense.date}
        ></ExpenseItem>
      ))}
      ...
    </div>
  );
}
```

## Using Stateful Lists

有了資料與頁面互相綁定後，接著要將新增項目 NewExpense 實際去更改資料，藉此更新頁面 ; 首先，我們可以接初始值 (一般來自於後端 API) 放置到 Component Function 之外，反正它只是暫時固定不變的值 (DUMMY_EXPENSES)，並透過 `useState` 建立觸發 Component Rerender 的資料，再將新增的資料塞到這組 Array 之中

```js
// App.js

const DUMMY_EXPENSES = [
  { title: "Car Insurance", amount: 294.67, date: new Date(2022, 6, 4) },
  { title: "Toilet Paper", amount: 94.12, date: new Date(2022, 6, 3) },
  { title: "New TV", amount: 299.49, date: new Date(2022, 6, 2) },
  { title: "New Desk (Wooden)", amount: 450, date: new Date(2022, 6, 1) },
];

const App = () => {
  const [expenses, setExpenses] = useState(DUMMY_EXPENSES);

  const addExpenseHandler = (expense) => {
    setExpenses([expense, ...expenses]);
  };

  return (
    <div className="App">
      <NewExpense onAddExpense={addExpenseHandler}></NewExpense>
      <Expenses items={expenses} />
    </div>
  );
};
```

## Updating State That Depends On The Previous State

看似已經完成了，但這裡有一個問題是，在前面的課程中有提到，如果更改 State 資料是建立在上一筆資料，那麼 setState 中就必須透過一個一組 callback function 來改動否則可能會沒有取得最新的上一筆資料

```js
const App = () => {
  const [expenses, setExpenses] = useState(DUMMY_EXPENSES);

  const addExpenseHandler = (expense) => {
    setExpenses(() => {
      return [expense, ...expenses];
    });
  };

  return (
    <div className="App">
      <NewExpense onAddExpense={addExpenseHandler}></NewExpense>
      <Expenses items={expenses} />
    </div>
  );
};
```

## Understanding "Keys"

目前為止，一切看似符合我們的需求了，但在 devtools console 中，卻出現一組 error log 如下:

![](key.png)

原因是在 React 中，如果是透過列表資料 (Array) 來渲染畫面，就必須在 JSX 中加入 `key` 屬性，藉此讓 React 知道每一筆資料所對應的 HTML Element，否則在每次更改這份列表資料時，React 因為無法知道每一筆資料所對應的 HTML Element，就會重新渲染整個列表，而非單純重新渲染更改的部分而已 ; 這不只會造成效能上的消耗，更有可能產生無法預期的 bug，例如在原有 item 上有其它的更動。

這點可以透過打開 devtools Elements 中發現，如果沒有加上 `key`，即便在資料列表中，我們是將新資料加在最前方，但卻是在最後渲染一組新的 HTML Element，並將前面每一組資料向後遞延一位，這是因為 React 無法得知哪一筆是新資料，所以只能將整組列表重新渲染。

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[Github](https://github.com/Jerry-Yeh/react-complete-guide/commit/f1ac375e92b665a95e430071c02ee59ee1871bc2)
