---
title: Dealing with Form Submission & Getting User Input Values
abbrlink: 16490
date: 2022-07-17 11:45:27
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

在先前的章節中，許多使用者操作都是透過表單來互動，接下來將進一步介紹表單的開發方式與驗證

<!-- more -->

## Getting Input Values

經歷了先前的課程，我們知道有兩種方式可以取得表單欄位資料

### useState() & onChange

第一種是透過監聽每次欄位的變動來更新 State 的值，這種方式的好處是可以隨時得到最新 State，但缺點也顯而易見，那就是持續監聽所帶來的效能消耗

```js
const SimpleInput = () => {
  const [enteredName, setEnteredName] = useState('');

  const nameInputChangeHandler = event => {
    setEnteredName(event.target.value);
  };

  return (
    <form>
      <div className="form-control">
        <label htmlFor="name">Your Name</label>
        <input type="text" id="name" onChange={nameInputChangeHandler} />
      </div>
    </form>
  );
};
```

### useRef() & onSubmit

另一種則是透過 `useRef()` 取得 input element，在送出表單時，一次性地取得欄位資料 ; 比較特別的是，HTML form submit 預設會送出 http request 給 server，並重新載入頁面，如果要避免這種情況就得透過原生 `event.preventDefault()` 來取消預設事件

```js
const SimpleInput = () => {
  const nameInputRef = useRef();

  const formSubmissionHandler = event => {
    event.preventDefault();
    
    console.log(nameInputRef.current.value);
  };

  return (
    <form onSubmit={formSubmissionHandler}>
      <div className="form-control">
        <label htmlFor="name">Your Name</label>
        <input ref={nameInputRef} type="text" id="name" onChange={nameInputChangeHandler} />
      </div>
    </form>
  );
};
```

這種方式有一個不適合的使用時機是，如果要在送出表單後，清空欄位資料(改變欄位資料)，就只能直接透過原生方法賦予欄位新的值

```js
const formSubmissionHandler = event => {
  event.preventDefault();

  nameInputRef.current.value = '';
};
```

這種作法在技術上可行，但實際上並不建議這麼做，原因是當專案已經透過 React 開發時，我們會希望完全交由 React 來操作 DOM，如果直接對操作 DOM 可能會導致無法預期的問題發生。

## 

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
