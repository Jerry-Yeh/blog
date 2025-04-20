---
title: How Component Function Are Excuted
abbrlink: 28796
date: 2022-06-05 22:23:07
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

一般透過使用者的操作進一步更改介面資料是非常普遍的行為，這一章節將透過 click event 來了解 Component Function 是如何運作的 

<!-- more -->

首先，我們將傳進 Component 的資料 `props.title` 透過一個變數存起來，接著再透過一個 button click event 來更改這筆資料，如下:

```js
// ExpenseItem.js
import ExpenseDate from "./ExpenseDate";
import "./ExpenseItem.css";
import Card from '../UI/Card';

const ExpenseItem = (props) => {
  let title = props.title;
  const clickHandler = () => {
    title = 'Updated !!!';
    console.log(title);
  };

  return (
    <Card className="expense-item">
      <ExpenseDate date={props.date} />
      <div className="expense-item__description">
        <h2>{title}</h2>
        <div className="expense-item__price">${props.amount}</div>
        <button onClick={clickHandler}>Change Title</button>
      </div>
    </Card>
  );
}

export default ExpenseItem;
```

然後我們嘗試透過點擊 button 來觸發資料更新，會發現 `console.log` 中的值有被印出來，但畫面上的資料並未被更改，為什麼會發生這樣的情況呢 ?

讓我們進一步了解 Component Function 的觸發方式，首先，會從最根本的檔案 (index.js) 開始運作，再從被指定做為 Root Component (App.js) 依序沿著被引入的 Component (Expenses.js -> ExpenseItem.js -> ExpenseDate.js) 所回傳的 JSX 取得頁面所需元件，最後再將它們渲染成瀏覽器可以讀取的 HTML 和 JavaScript，所有渲染的動作就到這裡為止，之後使用者所點擊的 click event 也只是呼叫我們所撰寫的 function，並不會再次渲染頁面，這也就是為什麼 `console.log` 中有印出資料，而頁面沒有跟著改變的原因了。

## Working with "State" (useState)

為了解決資料改動後，需重新渲染畫面的問題，React 提供了一個名為 `State` 的 React Hook，在使用上，React 會在前方加上 use (useState)，我們只需要透過 import react 來取得這個 function，而這個 function 只能在 Component Function 內"直接"被使用，不能在它之外，也不能在它的 method 之內

```js
// ExpenseItem.js
import ExpenseDate from "./ExpenseDate";
import { useState } from "react";
import "./ExpenseItem.css";
import Card from "../UI/Card";

useState();   // Error
const ExpenseItem = (props) => {
  let title = props.title;
  useState();   // Currect
  const clickHandler = () => {
    useState();   // Error
    title = "Updated !!!";
    console.log(title);
  };

  return (
    <Card className="expense-item">
      <ExpenseDate date={props.date} />
      <div className="expense-item__description">
        <h2>{title}</h2>
        <div className="expense-item__price">${props.amount}</div>
        <button onClick={clickHandler}>Change Title</button>
      </div>
    </Card>
  );
};

export default ExpenseItem;
```

否則會出現以下 error log

![](error-log.png)

當我們呼叫 `useState` 時，它會回傳一個 array，其中包含兩個值，分別為:

- 第一個 variable: 用來存放值的變數，以上方範例就是存放 `props.title` 這個傳進 Component 的值
- 第二個 function: 需要更改第一個變數的值時，可以透過呼叫它來改動，並在它的第一個參數代入要改動的值

那為什麼我們不能直接單純透過一個等號 = 賦予新的值呢 ? 原因是如果這麼做，又會回到一開始的狀況，值被更改但 React 並沒有重新渲染畫面，需要透過呼叫這個 function 更新，React 才會知道要重新呼叫 Component Function 以渲染畫面

既然回傳的是一個 array，這裡可以直接使用 ES6 Array Distructuring 來建立回傳 array 中的兩個參數

```js
// ExpenseItem.js
import ExpenseDate from "./ExpenseDate";
import { useState } from "react";
import "./ExpenseItem.css";
import Card from "../UI/Card";

const ExpenseItem = (props) => {
  const [title, setTitle] = useState(props.title);
  const clickHandler = () => {
    setTitle("Updated !!!")
    console.log(title);
  };

  return (
    <Card className="expense-item">
      <ExpenseDate date={props.date} />
      <div className="expense-item__description">
        <h2>{title}</h2>
        <div className="expense-item__price">${props.amount}</div>
        <button onClick={clickHandler}>Change Title</button>
      </div>
    </Card>
  );
};

export default ExpenseItem;
```

## A Closer Look at the "useState" Hook

即使是同一個 Component 在多處被重複使用，其內部的 State 都是獨立運作的彼此並不會互相干擾，否則我們當我們只是要操作單一 Component 的時候，所有使用到地方都同步更新就不符合我們期待的效果了，我們可透過在 child component 中加入 `console.log('ExpenseItem evaluated by React!')` 來判斷被呼叫的時機，從這點可以發現，只有在一次首次建立頁面時，被呼叫了 4 次，之後透過 button click event 觸發時都會只有一次

```js
// Expenses.js

import ExpenseItem from "./ExpenseItem";
import "./Expenses.css";
import Card from '../UI/Card';

const Expense = (props) => {
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

// ExpenseItem.js
import ExpenseDate from "./ExpenseDate";
import { useState } from "react";
import "./ExpenseItem.css";
import Card from "../UI/Card";

const ExpenseItem = (props) => {
  const [title, setTitle] = useState(props.title);
  console.log('ExpenseItem evaluated by React!');
  const clickHandler = () => {
    setTitle("Updated !!!")
    console.log(title);
  };

  return (
    <Card className="expense-item">
      <ExpenseDate date={props.date} />
      <div className="expense-item__description">
        <h2>{title}</h2>
        <div className="expense-item__price">${props.amount}</div>
        <button onClick={clickHandler}>Change Title</button>
      </div>
    </Card>
  );
};

export default ExpenseItem;
```

### const

那麼這裡又有另一個疑問了，既然透過 `useState` 所建立的變數是可以被更改的，那為什麼會使用 `const` 來建立呢 ?

原因是 React 在更新透過 `useState` 所建立的變數時，並不是單純透過等號 = 賦值，而是透過這個 Hook 對 React 進行註冊，所以當我們透過 `setTitle` 來更新時，React 會整個重新呼叫 Component Function，包含註冊變數這一行也會

```js
const [title, setTitle] = useState(props.title);
```

而 `props.title` 這個預設值並不會在更新時再次以它為預設值，只有第一次建立時才會，React 會對每次更新都透過 snapshot 的方式記錄，並在我們呼叫時回傳最新的值，藉此達到使用者與頁面互動的目的。

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[Github](https://github.com/Jerry-Yeh/react-complete-guide/commit/63ec319041dd1185e4f343c216cf66854cec40a2)
