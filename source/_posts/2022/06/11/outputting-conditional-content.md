---
title: Outputting Conditional Content
abbrlink: 42263
date: 2022-06-11 22:02:33
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

<!-- @format -->

這章節將透過篩選欄位來過濾符合條件的資料

<!-- more -->

其實在篩選條件的原理上和新增大同小異，都是對資料列表做出對應的改動，但比較不同的是，篩選時我們一般不會直接改動原資料，而是透過 `Array.filter()` 來產生一組符合條件的資料列表

```js
function Expenses(props) {
  const [filteredYear, setFilteredYear] = useState("2020");

  const filterExpenses = props.item.filter((item) => item.date.getFullYear() === +filteredYear);

  return (
    {filterExpenses.map((expense) => (
      <ExpenseItem
        key={expense.id}
        title={expense.title}
        amount={expense.amount}
        date={expense.date}
      ></ExpenseItem>
    ))}
  );
}
```

## 無資料預設內容

一般在介面設計上，無倫是圖片或是文字內容，在沒有取得資料時常會有預設內容以提升使用體驗，在這個範例中我們可以在判斷該年度沒有取得資料時，填入一段預設文字，如下:

```js
function Expenses(props) {
  const [filteredYear, setFilteredYear] = useState("2020");

  const filterExpenses = props.item.filter((item) => item.date.getFullYear() === +filteredYear);

  return (
    {filterExpenses.length === 0 ? (
      <p>No expenses found.</p>
    ) : (
      filterExpenses.map((expense) => (
        <ExpenseItem
          key={expense.id}
          title={expense.title}
          amount={expense.amount}
          date={expense.date}
        ></ExpenseItem>
      ))
    )}
  );
}
```

又或者可以透過 JavaScript &`&` 來改寫，當判斷第一個條件為 `true` 時，就顯示第二個條件的結果

```js
return (
  ...
  {filterExpenses.length === 0 && <p>No expenses found.</p>}
  {filterExpenses.length > 0 &&
    filterExpenses.map((expense) => (
      <ExpenseItem
        key={expense.id}
        title={expense.title}
        amount={expense.amount}
        date={expense.date}
      ></ExpenseItem>
    ))
  }
  ...
);
```

## 區分 JS 邏輯與 HTML 頁面

這樣的寫法有一個問題是，隨著我們開發內容不斷增加，如果把過多的邏輯都寫到 `return` 中，就會讓程式碼看起來相對複雜一點，為了避免這個情況，我們可以將邏輯從 `return` 中抽離到 Component Function ; 在 React 中，有一個比較特別的用法是，JSX 其實不只可以寫在 `return` 內，還可以讓它作為一個變數的值來儲存，透過這個功能我們就可以輕易地將邏輯獨立出來

```js
function Expenses(props) {
  const [filteredYear, setFilteredYear] = useState("2020");

  const filterExpenses = props.item.filter((item) => item.date.getFullYear() === +filteredYear);

  let expenseContent = <p>No expenses found.</p>;

  if (filterExpenses.length > 0) {
    expenseContent = filterExpenses.map((expense) => (
      <ExpenseItem
        key={expense.id}
        title={expense.title}
        amount={expense.amount}
        date={expense.date}
      ></ExpenseItem>
    ));
  }

  return (
    ...
    {expenseContent}
    ...
  );
}
```

## Adding Conditional Return Statements

我們可以進一步將這些內容抽離出成為一個 Component ExpenseList，如下:

```js
// Expenses.js
import ExpenseList from './ExpenseList';

function Expenses() {
  ...
  return {
    <ExpenseList items={filteredExpenses}/>
  };
}
// ExpenseList.js
import ExpenseItem from "./ExpenseItem";

function ExpenseList(props) {
  return (
    <ul>
      {props.items.map((expense) => (
        <ExpenseItem
          key={expense.id}
          title={expense.title}
          amount={expense.amount}
          date={expense.date}
        ></ExpenseItem>
      ))}
    </ul>
  );
}

export default ExpenseList;
```

但這裡有一個比較特別的做法是，因為 Component 抽離使的功能更加單純，所以先前透過判斷資料列表沒有資料時，所做的預設內容可以直接在條件符合時將之回傳

```js
function ExpenseList(props) {
  if (props.items.length === 0) {
    return <h2 className="expenses-list__fallback">Found no expenses.</h2>;
  }
  return (
    <ul className="expense-list">
      {props.items.map((expense) => (
        <ExpenseItem
          key={expense.id}
          title={expense.title}
          amount={expense.amount}
          date={expense.date}
        ></ExpenseItem>
      ))}
    </ul>
  );
}
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
