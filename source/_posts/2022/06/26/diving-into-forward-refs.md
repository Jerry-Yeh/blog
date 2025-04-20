---
title: Diving into Forward Refs
abbrlink: 61058
date: 2022-06-26 16:32:54
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

<!-- @format -->

在 React 中，如果要透過 `useRef()` 取得 component 內的 element，並操作它執行一些行為，如 focus、activate 等，就會需要特殊的處理

<!-- more -->

在需求方面，我們希望在點選表單送出按鈕時，如果有欄位驗證沒有通過，可以 focus 到該欄位，以供使用者更改輸入內容。

首先，先將 form 表單內重複出現的 input 欄位進行封裝，如下:

```js
// Login.js

const Login = (props) => {
  return (
    <Card className={classes.login}>
      {/* {emailState.value} */}
      <form onSubmit={submitHandler}>
        <Input
          ref={emailInputRef}
          isValid={emailIsValid}
          id="email"
          type="email"
          label="E-mail"
          value={emailState.val}
          onChange={emailChangeHandler}
          onBlur={validateEmailHandler}
        />
        <Input
          ref={passwordInputRef}
          isValid={passwordIsValid}
          id="password"
          type="password"
          label="Password"
          value={passwordState.val}
          onChange={passwordChangeHandler}
          onBlur={validatePasswordHandler}
        />
        <div className={classes.actions}>
          <Button type="submit" className={classes.btn}>
            Login
          </Button>
        </div>
      </form>
    </Card>
  );
};

// Input.js
import React from "react";

const Input = (props) => {
  return (
    <div
      className={`${classes.control} ${
        props.isValid === false ? classes.invalid : ""
      }`}
    >
      <label htmlFor={props.id}>{props.label}</label>
      <input
        type={props.type}
        id={props.id}
        value={props.value}
        onChange={props.onChange}
        onBlur={props.onBlur}
      />
    </div>
  );
};

export default Input;
```

## 錯誤方法一

遇到這種需求，可能很直覺的會直接透過 `useRef()` 取得 input 欄位，再透過 `useEffect()` 觸發預期的效果

```js
const Input = (props) => {
  const inputRef = useRef();

  useEffect(() => {
    inputRef.current.focus();
  }, []);

  return (
    <div
      className={`${classes.control} ${
        props.isValid === false ? classes.invalid : ""
      }`}
    >
      <label htmlFor={props.id}>{props.label}</label>
      <input
        ref={inputRef}
        type={props.type}
        id={props.id}
        value={props.value}
        onChange={props.onChange}
        onBlur={props.onBlur}
      />
    </div>
  );
};
```

但很不幸的是，透過這種直接綁定的方式，還是無法觸發 child component 內的 `useEffect()`

## 錯誤方法二

那如果嘗試在 parent component 透過 `useRef()` 綁定 component，並呼叫由 child component 提供的方法，如下:

```js
// Input.js
const Input = (props) => {
  const inputRef = useRef();

  const activate = () => {
    inputRef.current.focus();
  };

  return (
    <div
      className={`${classes.control} ${
        props.isValid === false ? classes.invalid : ""
      }`}
    >
      <label htmlFor={props.id}>{props.label}</label>
      <input
        ref={inputRef}
        type={props.type}
        id={props.id}
        value={props.value}
        onChange={props.onChange}
        onBlur={props.onBlur}
      />
    </div>
  );
};

// Login.js
const Login = (props) => {
  const emailInputRef = useRef();
  const passwordInputRef = useRef();

  const submitHandler = (event) => {
    event.preventDefault();
    // props.onLogin(enteredEmail, enteredPassword);
    if (formIsValid) {
      authCtx.onLogin(emailState.value, passwordState.value);
    } else if (!emailIsValid) {
      emailInputRef.current.activate();
    } else {
      passwordInputRef.current.activate();
    }
  };

  return (
    <Card className={classes.login}>
      <form onSubmit={submitHandler}>
        <Input
          ref={emailInputRef}
          isValid={emailIsValid}
          id="email"
          type="email"
          label="E-mail"
          value={emailState.val}
          onChange={emailChangeHandler}
          onBlur={validateEmailHandler}
        />
        <Input
          ref={passwordInputRef}
          isValid={passwordIsValid}
          id="password"
          type="password"
          label="Password"
          value={passwordState.val}
          onChange={passwordChangeHandler}
          onBlur={validatePasswordHandler}
        />
        <div className={classes.actions}>
          <Button type="submit" className={classes.btn}>
            Login
          </Button>
        </div>
      </form>
    </Card>
  );
};
```

結果還是沒有預期的效果

## 解決方式

其實到方法二的實作為止並沒有錯誤，只是還缺少了兩個步驟來解決這個問題

### useImperativeHandle()

第一步會需要用到一組新的 React Hooks - `useImperativeHandle()`，這組 hooks 有兩個參數

- ref
  這個參數需要帶入的是 component function 的二個參數 `ref`，如果在 parent component 有透過 `useRef()` 綁定 component，就可以透過這個參數建立連結

- callback function
  這個 function 會回傳一個 object，其中的 key 代表 parent component 透過 `useRef()` 綁定所能呼叫的方法，value 則是 child component 被呼叫後將執行的行為

```js
// Input.js
const Input = (props, ref) => {
  const inputRef = useRef();

  const activate = () => {
    inputRef.current.focus();
  };

  useImperativeHandle(ref, () => {
    return {
      focus: activate,
    };
  });

  return (
    <div
      className={`${classes.control} ${
        props.isValid === false ? classes.invalid : ""
      }`}
    >
      <label htmlFor={props.id}>{props.label}</label>
      <input
        ref={inputRef}
        type={props.type}
        id={props.id}
        value={props.value}
        onChange={props.onChange}
        onBlur={props.onBlur}
      />
    </div>
  );
};
```

### React.forwardRef

最後，如果是這種綁定 child component 特定 element 的需求，還會需要一個步驟是透過 `React.forwardRef` 來建立這個 child component

```js
const Input = React.forwardRef((props, ref) => {
  const inputRef = useRef();

  const activate = () => {
    inputRef.current.focus();
  };

  useImperativeHandle(ref, () => {
    return {
      focus: activate,
    };
  });

  return (
    <div
      className={`${classes.control} ${
        props.isValid === false ? classes.invalid : ""
      }`}
    >
      <label htmlFor={props.id}>{props.label}</label>
      <input
        ref={inputRef}
        type={props.type}
        id={props.id}
        value={props.value}
        onChange={props.onChange}
        onBlur={props.onBlur}
      />
    </div>
  );
});
```

### React.ForwardRefRenderFunction

可以做為 Component Function 參數放入 `React.forwardRef` 中

```ts
const Input = React.ForwardRefRenderFunction<HTMLDivElement, InputProps> = (props: InputProps, ref) => {
  ...

  return ...;
};
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[GitHub](https://github.com/Jerry-Yeh/React-The-Complete-Guide-Section9/commit/f384875ab26fe41ebb133cb34a20401491f5e054)
