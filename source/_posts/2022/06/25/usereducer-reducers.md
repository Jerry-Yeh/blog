---
title: useReducer & Reducers
abbrlink: 40125
date: 2022-06-25 12:04:31
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

隨著開發的功能和複雜度的逐漸提升，一個功能可能會由多組 State 來控制操作狀態，但因為 State 的更新機制，在某些情況下可能造成維護不易或預料之外的 Bug，所以 React 提供了另一個 Hook - `useReducer()` 來解決這種情況下的問題 

<!-- more -->

如果是一個簡單的表單驗證功能，透過 `useState()` 會需要個別有一組資料和驗證加以管理

```js
const Login = props => {
  const [enteredEmail, setEnteredEmail] = useState("");
  const [emailsValid, setEmailsValid] = useState("");
  ...

  const emailChangeHandler = event => {
    setEnteredEmail(event.target.value);
  };

  const validateEmailHandler = () => {
    setEmailsValid(enteredEmail.includes("@"));
  };

  const submitHandler = event => {
    event.preventDefault();
    ...
  };

  return (
    <form onSubmit={submitHandler}>
      <div
        className={`${classes.control} ${
          emailIsValid === false ? classes.invalid : ""
        }`}
      >
        <label htmlFor="email">E-Mail</label>
        <input
          type="email"
          id="email"
          value={enteredEmail}
          onChange={emailChangeHandler}
          onBlur={validateEmailHandler}
        />
      </div>
      ...
    </form>
  );
};
```

## useReducer()

在上述的範例中，可以發現其實這兩個 State 可以看作為一組，因為驗證的 State 還是會透過資料的 State 來判斷以回應對應的值，這種情況就可以透過 `useReducer()` 將之改寫為一組 State

首先，介紹一下其結構，在前方回傳的變數其實和 `useState()` 相同，分別為 State 和修改 State 的 `dispatchFn`，比較特別的呼叫 `useReducer()` 所帶入的三個參數

```js
const [state, dispatchFn] = useReducer(reducerFn, initialState, initFn);
```

- reducerFn(prevState, action): 
  因為其中的值並不會和 component function 內的其他值有交互作用，所以甚至可以把這個 function 寫在 component function 之外
  - prevState: 顧名思義就是"前一個"最新的 State snapshot
  - action: 藉由 `dispatchFn` 取得最新的 State snapshot
- initialState: State 初始值
- initFn: 將 State 設為初始值

透過 `useReducer()`，我們可以讓 State 為一個 object，並有兩個 key 分別表示值和驗證狀態，呼叫 `dispatchFn()` 時，可以單純帶入值和行為類別，將邏輯由 `reducerFn` 來處理

```js
const emailReducer = (prevState, action) => {
  if (action.type === "USER_INPUT") {
    return { value: action.val, isValid: action.val.includes("@") };
  } else if (action.type === "INPUT_BLUR") {
    return { value: prevState.value, isValid: prevState.value.includes("@") };
  }

  return { value: "", isValid: false };
};

const Login = props => {
  const [emailState, dispatchEmail] = useReducer(emailReducer, {
    value: "",
    isValid: false
  });
  ...

  const emailChangeHandler = event => {
    dispatchEmail({
      type: 'USER_INPUT',
      val: event.target.value
    });
  };

  const validateEmailHandler = () => {
    dispatchEmail({
      type: 'INPUT_BLUR',
      val: emailState.value
    });
  };

  ...

  return (
    <form onSubmit={submitHandler}>
      <div
        className={`${classes.control} ${
          emailState.isValid === false ? classes.invalid : ""
        }`}
      >
        <label htmlFor="email">E-Mail</label>
        <input
          type="email"
          id="email"
          value={emailState.value}
          onChange={emailChangeHandler}
          onBlur={validateEmailHandler}
        />
      </div>
      ...
    </form>
  );
};
```

但這裡要注意的是，因為 `isValid` 初始值就是 `false`，所以一開始在 UI 上就會顯示驗證未通過的樣式，為了區分初始狀態，可已透過其他 falsy 值如 null, undefined 來代替

```js
const [emailState, dispatchEmail] = useReducer(emailReducer, {
    value: "",
    isValid: null or undefined
  });
```

## With useEffect()

有了個別欄位的驗證變數後，仍然會需要一組總表單的驗證變數來判斷每個欄位是否都驗證通過，這時候就可以透過先前的 `useEffect()` 來監聽所有欄位的變化

```js
useEffect(() => {
  const identifier = setTimeout(() => {
    setFormIsValid(
      emailState.isValid && passwordState.isValid
    );
  }, 500);

  return () => {
    clearTimeout(identifier);
  };
}, [emailState, passwordState]);
```

寫到這裡其實已經達到驗證功能的目的了，但還可以進一步優化的地方是，這樣的寫法在驗證上"過於頻繁"了，只要欄位的值一改動，就會進行監聽，其實需要判斷的時機只有在個別欄位的驗證結果發生改變時，才會需要更新總表單的驗證結果

```js
useEffect(() => {
  const identifier = setTimeout(() => {
    setFormIsValid(
      emailState.isValid && passwordState.isValid
    );
  }, 500);

  return () => {
    clearTimeout(identifier);
  };
}, [emailState.isValid, passwordState.isValid]);
```

或是可以透過 ES6 object de-structuring 來個別取的 `isValid` 的值

```js
const { isValid: emailValid } = emailState;
const { isValid: passwordValid } = passwordState;

useEffect(() => {
  const identifier = setTimeout(() => {
    setFormIsValid(
      emailValid && passwordValid
    );
  }, 500);

  return () => {
    clearTimeout(identifier);
  };
}, [emailValid, passwordValid]);
```

## useState() vs useReducer()

在 React 中，其實並沒有明確規定什麼時候該用哪一種 hook，兩者可以辦到的事相同，只是在什麼時機更適合，基本上可以為兩者的使用時機做出一些區別

- useState()

  - 一個主要由 State 來管理的工具
  - 每個 State 獨立運作，較沒有相互作用的情況
  - State 的更新較為單純，改別的值也比限制在固定的範圍內

- useReducer()

  - 更複雜的資料結構，如 object
  - 資料彼此之間的關聯性高，更新時環環相扣
  - 處理更複雜的邏輯

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[useReducer()](https://github.com/Jerry-Yeh/React-The-Complete-Guide-Section9/commit/62833e04b790178ecee12e6130f1d5aa2bcf7664)
[with useEffect()](https://github.com/Jerry-Yeh/React-The-Complete-Guide-Section9/commit/8d16b47603cf77097c9a90a8d409184bbeca57f7)