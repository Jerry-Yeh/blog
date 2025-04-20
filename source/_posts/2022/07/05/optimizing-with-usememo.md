---
title: Optimizing with useMemo()
abbrlink: 38306
date: 2022-07-05 22:41:45
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

<!-- @format -->

在先前的章節中，介紹了透過 `React.memo()` 和 `useCallback()` 來解決父子元件資料傳遞上，非必要 re-render 的問題，而這章節將介紹另一個 hook - `useMemo()` 來對子元件本身效能上的優化

<!-- more -->

`useMemoe()` 與 `React.memo()` 的差異在於 `useMemo()` 著重於避免元件內複雜的邏輯被非必要的重複執行

一組簡單的範例如下，一個 component DemoList 有一個數字陣列，透過一個 sort function 將其中的值依大小排列，接著在父元件導入這個 component

```js
// DemoList.js
const DemoList = () => {
  const [listItems, setListItems] = useState([5, 3, 1, 10, 9]);

  const sortedList = () => {
    console.log('Items sorted');
    return listItems.sort((a, b) => a - b);
  };

  console.log('DEMOLIST RUNNING');

  return (
    <div>{sortedList().map(item => (
      <li key={item}>{item}</li>
    ))}</div>
  );
};

// App.js
const App = () => {
  const [allowToggle, setAllowToggle] = useState(false);

  const allowToggleHandler = () => {
    setAllowToggle(true);
  };


  return (
    <DemoList />
    <Button onClick={allowToggleHandler}>Allow Toggling</Button>
  );
};
```

如果沒有別處理，在點擊 Allow Toggling 按鈕時，因為 State 改變，所以 DemoList 也會 re-render，自然會再次執行 sort function;如果要避免這種情況，就可以透過 `useMemo()` 將 sort function 寫入，並在第二個參數 dependency array 中放入 State listItems，這麼一來就只有在 dependency 發生變動時才會再次執行 sort function

```js
const sortedList = useMemo(() => {
  console.log("Items sorted");
  return listItems.sort((a, b) => a - b);
}, [listItems]);
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[GitHub](https://github.com/Jerry-Yeh/Jerry-Yeh-React-The-Complete-Guide-Section12/commit/4d7332f6e034ad8ec7f4bfbe94a9b3cb17d02eaa)
