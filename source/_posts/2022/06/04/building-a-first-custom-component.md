---
title: Building a First Custom Component
abbrlink: 5794
date: 2022-06-04 12:12:49
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

這堂課將介紹如何客製化一個 Component

<!-- more -->

## Building a Basic Component

延續先前的環境，其實在預設環境中，就有一個名為 APP 的 Component，這個 Component 是整個 SPA 專安的 Root Component，之後所有開發的 Component 都會從中做延伸。

而這個 Component 也為我們示範了如何在 React 中建立一個 Component，其實就是透過 JavaScript function return HTML Element (JSX) 來達到建立 Component 的目的 ; 我們一般會在 src 路徑底下建立一個 components 資料夾，接著在其內建立 component，如下，是一個名為 ExpenseItem 的 component，通常我們直接運用 Component 的名稱作為 return function 的名稱即可

```
src
  - components
    -ExpenseItem
  - App.js
```

```js
// ExpenseItem.js
function ExpenseItem() {
  return <h2>Expense item!</h2>;
}

export default ExpenseItem;
```

接著會在 App.js 這個 Root Component 引入它，一般而言，我們會將 React Component 以 uppercase character 的方式寫入，以便區分原生 HTML element 的 lowercase character

```js
// App.js
import ExpenseItem from './components/ExpenseItem';

function App() {
  return (
    <div>
      <h2>Let's get started!</h2>
      <ExpenseItem></ExpenseItem>
    </div>
  );
}

export default App;
```

最後就可以在畫面上看到這些引入的內容了

## Writing More Complex JSX Code

當然一般的開發情況不會像上面這個 Component 這麼單純，一個 Component 常常會有更多內容需要呈現，這時候我們需要透過小括號 () 來將這些 HTML element 包起來，並透過一個 `div` 將所有內容放進去，確保回傳時只有一個 parent element

```js
// ExpenseItem.js
function ExpenseItem() {
  return (
    <div>
      <div>June 4 2022</div>
      <div>
        <h2>Car Insurance</h2>
        <div>$294.67</div>
      </div>
    </div>
  );
}

export default ExpenseItem;
```

## Adding Basic CSS Styling

當然一個完整的專案不會只有 HTML 和 JavaScript，還會有許多 CSS 來調整 UI 的呈現，在 React 中，我們通常會在 Component 旁直接建立一支同名的 CSS 來開發它的 style，而且只要單純透過 `import` 就可以使用，但這裡要特別注意的是，Component function 所 return 的 HTML 終究不是原生的 HTML Element，而是 React 團隊開發出來像是 HTML 的 JSX，因此在寫法上會有些許的不同，一般採用 CSS 所使用 `class` 屬性也要改成 `className`

```js
// ExpenseItem.js
function ExpenseItem() {
  return (
    <div className="expense-item">
      <div>June 4 2022</div>
      <div className="expense-item__description">
        <h2>Car Insurance</h2>
        <div className="expense-item__price">$294.67</div>
      </div>
    </div>
  );
}

export default ExpenseItem;
```

## Outputting Dynamic Data & Working with Expressions in JSX

到目前為止，Component 的值還是 hard coded 在 HTML 內，第一步我們需要讓它由動態的資料帶入，回到之前所說的，React Component 它就是一個 JavaScript function，所以我們可以先在 function 內透過變數來代指這些資料，而在 JSX 中，我們可以透過大括號 {} 來填入各種 JavaScript 程式碼，藉此帶入動態的資料

```js
// ExpenseItem.js
function ExpenseItem() {
  const expenseDate = new Date(2022, 6, 4);
  const expenseTitle = "Car Insurance";
  const expenseAmount = 294.67;

  return (
    <div className="expense-item">
      <div>{expenseDate.toISOString()}</div>
      <div className="expense-item__description">
        <h2>{expenseTitle}</h2>
        <div className="expense-item__price">${expenseAmount}</div>
      </div>
    </div>
  );
}

export default ExpenseItem;
```

## Passing Data via "props"

將整個專案拆成許多 Component 的目的不外乎就是為了能讓每個 Component 處理單一的邏輯與功能並提高程式碼的複用性 Reusable，因此，就以這個記帳項目的 Component 來說，資料應該是由外而內傳入的 Configurable ，並不會有固定的資料在裡面，否則就無法實現透過代入不同資料複用其功能與介面的目的了，Component 彼此無法直接使用對方的參數，這時候就會用到 React 中一個很重要的方法 - `props`。

首先，我們會有一筆資料列表位於外層的 Component (App.js)，接著可以自訂一名稱將資料變數直接定義在 JSX Component 上，並一樣透過大括號 {} 來代入這些資料變數

```js
// App.js

import "./App.css";
import ExpenseItem from "./components/ExpenseItem";

function App() {
  const expenses = [
    { title: "Car Insurance", amount: 294.67, date: new Date(2022, 6, 4) },
    { title: "Toilet Paper", amount: 94.12, date: new Date(2022, 6, 3) },
    { title: "New TV", amount: 299.49, date: new Date(2022, 6, 2) },
    { title: "New Desk (Wooden)", amount: 450, date: new Date(2022, 6, 1) },
  ];

  return (
    <div className="App">
      <h2>Let's get started!</h2>
      <ExpenseItem
        title={expenses[0].title}
        amount={expenses[0].amount}
        date={expenses[0].date}
      ></ExpenseItem>
      <ExpenseItem
        title={expenses[1].title}
        amount={expenses[1].amount}
        date={expenses[1].date}
      ></ExpenseItem>
      <ExpenseItem
        title={expenses[2].title}
        amount={expenses[2].amount}
        date={expenses[2].date}
      ></ExpenseItem>
      <ExpenseItem
        title={expenses[3].title}
        amount={expenses[3].amount}
        date={expenses[3].date}
      ></ExpenseItem>
    </div>
  );
}

export default App;
```

那麼接下來我們要如何取得傳進來的參數呢 ? 記得 Component 不過就是一個 return function，因此我們可以將參數透過 function parameter 傳入，但這裡要注意的是，無論我們在 Parent Component 中定義了多少個參數要傳到 Child Component 中，function parameter 都只會有一個，至於名稱可以自由定義，但一般我們會將它定義為這個方法的名稱 - `props`，而在 Parent Component 中定義於 JSX Component 上的名稱就是 `props` 的 `key`，如此一來就可以透過代入不同的參數達到 Component Reusable。

```js
import "./ExpenseItem.css";

function ExpenseItem(props) {
  return (
    <div className="expense-item">
      <div>{props.date.toISOString()}</div>
      <div className="expense-item__description">
        <h2>{props.title}</h2>
        <div className="expense-item__price">${props.amount}</div>
      </div>
    </div>
  );
}

export default ExpenseItem;
```

## Adding "normal" JavaScript Logic to Components

到目前為止，上述程式碼所渲染出來的日期格式與一般大家常見的使用習慣還是有點出入，如果我們希望日期可以被單獨拆分成年、月和日，可以使用 JavaScript Date 原生語法 [toLocaleString](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleString) 和 [getFullYear](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getFullYear) 來個別取得這些資料

```js
// ExpenseItem.js

import "./ExpenseItem.css";

function ExpenseItem(props) {
  return (
    <div className="expense-item">
      <div>
        <div>{props.date.toLocaleString("en-US", { month: "long" })}</div>
        <div>{props.date.toLocaleString("en-US", { day: "2-digit" })}</div>
        <div>{props.date.getFullYear()}</div>
      </div>
      <div className="expense-item__description">
        <h2>{props.title}</h2>
        <div className="expense-item__price">${props.amount}</div>
      </div>
    </div>
  );
}

export default ExpenseItem;
```

即便在 React JSX 中，我們可以在大括號 {} 寫入任何 JavaScript 邏輯，但為了 HTML 與 JavaScript 可以區分得更明確，我們可以透過變數儲存這些邏輯再帶入 JSX 中

```js
// ExpenseItem.js

import "./ExpenseItem.css";

function ExpenseItem(props) {
  const month = props.date.toLocaleString("en-US", { month: "long" });
  const day = props.date.toLocaleString("en-US", { day: "2-digit" });
  const year = props.date.getFullYear();

  return (
    <div className="expense-item">
      <div>
        <div>{props.date.toLocaleString("en-US", { month: "long" })}</div>
        <div>{props.date.toLocaleString("en-US", { day: "2-digit" })}</div>
        <div>{props.date.getFullYear()}</div>
      </div>
      <div className="expense-item__description">
        <h2>{props.title}</h2>
        <div className="expense-item__price">${props.amount}</div>
      </div>
    </div>
  );
}

export default ExpenseItem;
```

## Splitting Components into Multiple Components

一般在開發時，為了因應使用需求，專案往往會有越來越多的功能與 Component，這是非常正常的 ; 因此，為了讓每個 Component 負責的功能更加單純，我們會將原有的 Component 繼續拆分成更小的 Component，但這並沒有一個硬性的規定什麼時候應該拆分，單純依照需求和經驗來判斷，延續上方的範例，我們可以嘗試將 ExpenseItem 中的 date 拆分成一個 ExpenseDate 使其功能更加單純。

首先，從 ExpenseItem 拆分出去後，我們需要 import ExpenseDate 將其引入使用，並將接受到的 date 這筆資料再次透過 `props` 傳給它，這裡特別注意一下，即便 ExpenseItem 因將 date 功能拆分出去後，並沒有直接使用到 date 這筆資料，但仍需透過它再將這筆資料 `props` 給 ExpenseDate，因為在 React 中，如果透過 `props` 傳遞資料，我們無法"跳過"中間層的 Component

```js
// ExpenseItem.js
import ExpenseDate from "./ExpenseDate";
import "./ExpenseItem.css";

function ExpenseItem(props) {
  return (
    <div className="expense-item">
      <ExpenseDate date={props.date} />
      <div className="expense-item__description">
        <h2>{props.title}</h2>
        <div className="expense-item__price">${props.amount}</div>
      </div>
    </div>
  );
}

export default ExpenseItem;

// ExpenseDate.js
function ExpenseDate(props) {
  const month = props.date.toLocaleString("en-US", { month: "long" });
  const day = props.date.toLocaleString("en-US", { day: "2-digit" });
  const year = props.date.getFullYear();

  return (
    <div>
      <div>{month}</div>
      <div>{day}</div>
      <div>{year}</div>
    </div>
  );
}

export default ExpenseDate;
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[Github](https://github.com/Jerry-Yeh/react-complete-guide/commit/f2bd63783737eb58e311d7ecd36d476baaebc7d8)
