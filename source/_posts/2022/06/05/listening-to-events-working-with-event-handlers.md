---
title: Listening to Events & Working with Event Handlers
abbrlink: 41269
date: 2022-06-05 16:01:40
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

到目前為止，專案的實作已拆成許多 Component 使其功能更加單純且可以重覆使用，但都還只停留在靜態資料，沒有包含使用者操作的行為，接下來將透過 React 提供的方式加入這些行為。

<!-- more -->

在 React 中，我們完全可以使用許多 [JavaScript Event](https://developer.mozilla.org/zh-TW/docs/Web/API/HTMLElement)，但 JSX 並非一般 HTML，因此在寫法上會有些許的不同，如同前面課程所使用到的，`class` 就會由 `className` 來改寫，而 event 則會改由 `on${Event}` 這種前方加上 on 的 Capital Character 的寫法，這裡我們先單純加上一個按鈕並透過 click event 印出一些資訊來測試

```js
// ExpenseItem.js
function ExpenseItem() {
  return (
    ...
    <button onClick={() => {console.log('Clicked !!!')}}>Change Title</button>
    ...
  );
}
```

當然，也並非一定要使用 ES6 Arrow Function 不可，只是相較於一般 function 寫法更加簡潔

```js
// ExpenseItem.js
function ExpenseItem() {
  return (
    ...
    <button onClick={function() {console.log('Clicked !!!')}}>Change Title</button>
    ...
  );
}
```

但就如同前面課程中所提到，在 JSX 中透過大括號 {} 可以寫入任何 JavaScript 邏輯，但為了更好的區分介面與邏輯，盡可能將邏輯寫在 JSX 外，再由名稱代入

```js
// ExpenseItem.js
function ExpenseItem() {
  const clickHandler = () => console.log('Clicked !!!');

  return (
    ...
    <button onClick={clickHandler}>Change Title</button>
    ...
  );
}
```

這裡要特別注意的是，由 click event 所觸發的 `clickHandler` function 不需要寫成 `clickHandler()`，因為我們只是要讓程式知道當 click event 被觸發時，要呼叫這個 function，而不是在渲染它時就呼叫。

另外，在命名上，有一個非強制的建議是可以透過 `${event}Handler` 這種命名方式來建立 function，以此區別單純由我們程式所觸發的 function 呼叫。

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[Github]()
