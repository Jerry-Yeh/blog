---
title: The Concept of Composition (children props)
abbrlink: 53422
date: 2022-06-05 10:46:40
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

<!-- @format -->

延續之前的範例，我們還可以將 Component 中 container 共同的特性獨立出來做為一個可複用的 Component，這就稱為 Composition

<!-- more -->

在先前的案例中，我們可以發現，Expenses.css 和 ExpenseItem.css 中有相同的卡片樣式，這些相同的樣式都是 Component container 樣式，如下:

```css
/* Expenses.css */
.expenses {
  padding: 1rem;
  background-color: rgb(31, 31, 31);
  margin: 2rem auto;
  width: 50rem;
  max-width: 95%;
  /* border-radius: 12px;
  box-shadow: 0 1px 8px rgba(0, 0, 0, 0.25); */
}

/* ExpenseItem.css */
.expense-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.5rem;
  margin: 1rem 0;
  background-color: #4b4b4b;
  /* border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.25); */
}
```

我們可以將這裡相同的樣式切割出來成為 另一個 Component Card.js & Card.css

```js
// Card.js
function Card() {
  return <div className="card"></div>;
}
```

```css
/* Card.css */
.card {
  border-radius: 12px;
  background-color: #4b4b4b;
}
```

接著將這個 Component 取代 Expenses.js & ExpenseItem.js 的 Container div element

```js
// Expenses.js

import ExpenseItem from "./ExpenseItem";
import "./Expenses.css";
import Card from './Card';

function Expense(props) {
  return (
    <Card className="expenses">
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
}

export default Expense;
```

## props.children

但在這裡會發現，Container 中的內容並沒有被渲染出來，但不用擔心，React 提供了一個解決方法，就是在 Component 中透過 `props.children` 作為內容代入 JSX 內，這個方法很像 Vue `slot` 的用法，就是透過一個標籤指定 Component 其他插入資訊的位置，如此一來就可以在預期的地方渲染出對應的畫面了

```js
// Card.js
import './Card.css';

function Card(props) {
  return (
    <div className="card">{props.children}</div>
  );
}

export default Card;
```

## props.className

到這裡為止，元件都有成功被渲染，但會發現原先 container 上的 className (expenses)並沒有被讀取到，而是單純只有因重複而被拉出去的 className (card) ; 這裡 React 提供了另一個方法 `props.className` 使兩者的 css 都可以被讀取到

```js
// Card.js
import './Card.css';

function Card(props) {
  const classes = 'card ' + props.className;
  return (
    <div className={classes}>{props.children}</div>
  );
}

export default Card;
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[Github](https://github.com/Jerry-Yeh/react-complete-guide/commit/0739aa61606b6c6a678e504ea2072cee2e9ccbe1e)
