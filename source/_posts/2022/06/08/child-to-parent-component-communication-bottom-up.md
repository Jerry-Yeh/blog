---
title: Child to Parent Component Communication (Bottom-up)
abbrlink: 9807
date: 2022-06-08 23:14:53
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

在過去的課程中，我們可以透過 `props` 將資料由上而下從父元件傳遞到子元件，接下來將介紹如何層子元件由下而上傳遞資料到父元件

<!-- more -->

首先，我們先來了解一下為什麼會有這種由下而上傳遞資料的需求呢 ? 原因一般我們在開發時，會將頁面拆分成許多 Component 以利功能更加單純並兼具複用性，所以常會有資料從一個 Component 生成，但要反映到另一個 Component 來呈現，但傳遞資料只能以 Component 之間的關係垂直傳遞，無法任意傳遞到其他 Component，否則資料就會過於混亂難以管理。

那麼實作上到底該怎麼執行呢 ? 其實答案在之前的範例中已經出現了，那就是我們在監聽原生 `input` 時，透過 `onChange` 綁定一個 function，當 function 被執行時，其第一個參數就是從 input 回傳的 event

```js
// ExpenseForm.js
function ExpenseForm() {
  const [enteredTitle, setEnteredTitle] = useState();
  
  const titleChangeHandler = (event) => {
    setEnteredTitle(event.target.value);
  };

  return (
    <form>
      ...
      <label>Title</label>
      <input
        type="text"
        value={enteredTitle}
        onChange={titleChangeHandler}
      />
      ...
    </form>
  );
}

```

回到我們的範例，首先，在父元件 (NewExpense) 綁定 event 到子元件 (ExpenseForm) 上，並期待它會回傳表單資訊 (expenseData)，再透過 `Math.random()` 追加一組隨機產生的 unique id (可能會重複，僅供練習測試)，

```js
// NewExpense.js
const NewExpense = () => {
  const enteredExpenseHandler = (expense) => {
    const expenseData = {
      ...expense,
      id: Math.random().toString(),
    };
    console.log('expenseData', expenseData);
  };

  return (
    <div className='new-expense'>
      <ExpenseForm onEnteredExpense={enteredExpenseHandler}/>
    </div>
  );
};
```

接著來到子元件，我們可以從 `props` 中取得傳進來的 event `onEnteredExpense`，並將表單資料 `expenseData` 回傳給它

```js
// ExpenseForm.js
function ExpenseForm(props) {
  ...
  const submitHandler = (event) => {
    event.preventDefault();

    const expenseData = {
      title: enteredTitle,
      amount: enteredAmount,
      date: new Date(enteredDate),
    };

    props.onEnteredExpense(expenseData);
    setEnteredTitle('');
    setEnteredAmount('');
    setEnteredDate('');
  };

  return (
    ...
  );
}
```

但需要這份表單資料的另一組元件 `ExpenseItem`，所以我們還需要繼續往上傳至根元件 `App.js`，才能過它傳遞到目標元件，而 Component 垂直傳遞的方式無法跳過中間元件 `NewExpense` (後面有其他作法)

```js
// NewExpense.js
const NewExpense = (props) => {
  const enteredExpenseHandler = (expense) => {
    const expenseData = {
      ...expense,
      id: Math.random().toString(),
    };
    props.onAddExpense(expenseData);
  };

  return (
    <div className='new-expense'>
      <ExpenseForm onEnteredExpense={enteredExpenseHandler}/>
    </div>
  );
};

// App.js
const App = () => {
  ...

  const addExpenseHandler = expense => {
    console.log('App.js', expense);
  };

  return (
    <div className="App">
      <NewExpense onAddExpense={addExpenseHandler}></NewExpense>
      <Expenses items={expenses} />
    </div>
  );
}
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[Github](https://github.com/Jerry-Yeh/react-complete-guide/commit/48949796ffcf7c04fa32de9e096e8d39a4b71af9)
