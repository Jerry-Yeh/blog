---
title: React Refs
abbrlink: 43584
date: 2022-06-19 10:10:02
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

在 React 中，可以透過 useState 在 component 重新選染時保留資料，並透過 handler & value 來為 JSX 與 JS 做到雙向綁定，但在某些情況下，會只有特定的時機會需要取得資料，並不需要隨時對資料進行監聽，因此 React 提供了一個更輕便的方式 - Refs 來達到這個目的

<!-- more -->

回到先前的範例，為了避免 component rendered 所造成的資料流失，我們會透過 state 來儲存在資料，並透過 handler 監聽資料的更動以即時更新

```js
const NewUser = props => {
  const [user, setUser] = useState({
    username: "",
    age: "",
  });

  const submitHandler = (event) => {};

  const usernameChangeHandler = () => {};

  const ageChangeHandler = () => {};

  return (
    ...
    <form onSubmit={submitHandler}>
      <label htmlFor="username">Username</label>
      <input
        id="username"
        type="text"
        value={user.username}
        onChange={usernameChangeHandler}
      />
      <label htmlFor="age">Age(Years)</label>
      <input
        id="age"
        type="number"
        value={user.age}
        onChange={ageChangeHandler}
      />
      <Button type="submit">Add User</Button>
    </form>
  );
};
```

## React Refs

但其實我們不需要隨時監聽資料的更動，只需要在送出表單時，取得當前資料即可，因此 React 便提供了另一個 hooks - Refs ; 如同其他的 React hooks，Refs 也只能應用於 component function 之中，透過查看 useRefs 所傳的值，它總是會包含一個屬性 - current，這個屬性即是我們綁定的 DOM，其中的 value 即是 DOM value。

```js
import {useRef} from 'react';

const NewUser = props => {
  const nameInputRef = useRef();

  const submitHandler = event => {
    console.log(nameInputRef);
  };

  const usernameChangeHandler = (event) => {...};

  return (
    <form onSubmit={submitHandler}>
      <label htmlFor="username">Username</label>
      <input
        id="username"
        type="text"
        value={user.username}
        onChange={usernameChangeHandler}
        ref={nameInputRef}
      />
      ...
    </form>
  );
};
```

![](refs-value.png)

透過 Refs，便可在送出表單時取得即時資料，因此先前綁定 State 所使用的監聽方式也不在必要，單純透過綁定 Refs 也可以直接更改 `current.value` 的值，如送出表單後清空欄位的功能

```js
const submitHandler = (event) => {
  ...
  nameInputRef.current.value = '';
  ageInputRef.current.value = '';
};

return (
  <form onSubmit={submitHandler}>
    <label htmlFor="username">Username</label>
    <input
      id="username"
      type="text"
      ref={nameInputRef}
    />
    ...
  </form>
);
```

## Controlled vs Uncontrolled Components

在 React 中，透過 State 綁定資料與 JSX，及時監聽 JSX 變化，並透過 React 回填最新的資料，這種完全由 React 所主導的元件稱之為 - Controlled Component。

```js
import {useState} from 'react';

const component = () => {
  const [username, setUsername] = useState();

  const submitHandler = event => {
    event.preventDefault();
    
    ...
    
    setUsername('');
  };

  const usernameHandler = event => {
    setUsername(event.target.value);
  };

  return (
    <form onSubmit={submitHandler}>
      <input value={username} onChnage={usernameHandler}/>
    </form>
  );
};

```

反之，透過 Refs 綁定 JSX 後，只在需要時即時查看最新資料，這種非完全透過 React 完全掌握資料變動的方式就稱為 - Uncontrolled component ; 但這裡也許會有些疑問是，在送出表單後，不也是透過更動 `useRef` 所回傳的 `object.current.value` 來達到主動更新資料嗎 ?

這裡的差異是，Ref 更新資料只是透過 DOM API 來更新 value，來不是完全由 React 提供的方式來辦到 ; 也許到目前為止還無法感受這麼區分的目的，但這在 React 的開發中是非常典型用來區分 component 開發模式的方式，在更進階的開發中會進一步探討它們的差異。

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
