---
title: State Scheduling & Batching
abbrlink: 60466
date: 2022-07-05 22:10:03
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

這章節將進一步套討 React State 更新與 Batch 策略 

<!-- more -->

在前面的章節中，反覆提到一個很重要的觀念就是，如果 State 發生變化，React 就會 re-render component，但時常在操作畫面時，可能一個動作就操作多筆 State，一瞬間多次 re-render 勢必會造成不必要的資源消耗，因此，React 預設使用了一個機制 - Batch Update 來解決這個問題。

在每次透過 setState 來更新 State 時，其實並不是在程式碼位置的當下"馬上"更新並 re-render component，只是因為電腦運作得非常快讓我們覺得是立即執行，而實際上其實會被放進 Schedule 等待執行，直到這個行為的 function 內所有的 setState 程式碼跑完後，React 會進行一次 Batch Update 一口氣更新所有 State，並只 re-render "一次"，進而省略不必要 re-render 的效能消耗 ; 但 Batch Update 有一個前提是這些 State Update 必須是 sync，如果是 async event 如 promise 或 setTimeout ...等，就無法被視為同一個 Schedule 一同執行。

一個簡單的範例如下，在一次的點擊事件中，update State 兩次

```js
const App = () => {
  const [counter, setCounter] = useState(0);

  const handleClick = () => {
    setCounter(counter + 1);
    setCounter(counter + 1);
  };

  return (
    <div className='App'>
      <h1>Function Component</h1>
      <div>
        counter: {counter}
      </div>
      <br/>
      <button onClick={handleClick}>Click me</button>
    </div>
  );
};
```

結果 counter 會是 1，因為這兩次的 State updated 會放進 schedule 而透過 batch update 一起執行，所以其實其中的 counter 都會是在更新前的 0，更新的動作就可以視為一模一樣的動作發生了兩次

```js
const handleClick = () => {
  setCounter(0 + 1);
  setCounter(0 + 1);
};
```

那麼如果要達到 +2 的目的，可以怎麼修改這段程式碼呢 ?

在先前的章節有提到，因為多組 State 更新時，可能會發生無法取得最新 State 的潛在問題，所以如果要明確取得 State 最新的值，在 setState 中就要改使用 callback function，其中的參數就會是最新的值

```js
const increase = prevCounter => prevCounter + 1;
const handlerClick = () => {
  setCOunter(increase);
  setCOunter(increase);
};
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[React 的 batch update 策略](https://lance.coderbridge.io/2021/06/10/react-batch-update-in-hooks-and-react18/)
