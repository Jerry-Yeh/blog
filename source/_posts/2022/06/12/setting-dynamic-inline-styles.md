---
title: Setting Dynamic Inline Styles
abbrlink: 18219
date: 2022-06-12 15:31:38
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

<!-- @format -->

在前一章節中，我們可以將變數代入 JSX style 中動態賦值，這堂課將進一步套討 Conditional Style 更好的應用方式

<!-- more -->

在一個基本的輸入欄位中，如果使用者未輸入任何訊息及送出表單，通常會有進一步提示以提醒操作方式有誤，一個簡易的表單如下，如果判斷欄位為空，則 `return` 不做資料上的更改

```js
const CourseInput = (props) => {
  const [enteredValue, setEnteredValue] = useState("");

  const goalInputChangeHandler = (event) => {
    setEnteredValue(event.target.value);
  };

  const formSubmitHandler = (event) => {
    event.preventDefault();
    if (enteredValue.trim().length === 0) return; // 欄位為空
    props.onAddGoal(enteredValue);
  };

  return (
    <form onSubmit={formSubmitHandler}>
      <div className="form-control">
        <label>Course Goal</label>
        <input type="text" onChange={goalInputChangeHandler} />
      </div>
      <Button type="submit">Add Goal</Button>
    </form>
  );
};
```

進一步如果需要加上提示，就需要動態修改 `style` 內容，如下:

```js
const CourseInput = (props) => {
  const [enteredValue, setEnteredValue] = useState("");
  const [isValid, setIsValid] = useState(true);

  const goalInputChangeHandler = (event) => {
    if (event.target.value.trim().length > 0) {
      setIsValid(true);
    }
    setEnteredValue(event.target.value);
  };

  const formSubmitHandler = (event) => {
    event.preventDefault();
    if (enteredValue.trim().length === 0) {
      setIsValid(false);
      return;
    }
    props.onAddGoal(enteredValue);
  };

  return (
    <form onSubmit={formSubmitHandler}>
      <div className="form-control">
        // New Style Start
        <label style={{ color: !isValid ? "red" : "black" }}>Course Goal</label>
        <input
          style={{
            borderColor: !isValid ? "red" : "#ccc",
            backgroundColor: !isValid ? "salmon" : "transparent",
          }}
          type="text"
          onChange={goalInputChangeHandler}
        />
        // New Style End
      </div>
      <Button type="submit">Add Goal</Button>
    </form>
  );
};
```

但很顯然的，這不是一個好方法，除了需要在 JSX 中加入大量的 style 和判斷外，在一般的狀態下還得重複撰寫已經定義在 .css 檔案中的初始樣式，在 React 中，我們有更好的方式來完成 Conditional Style

## Setting CSS Classed Dynamically

其實無論是否透過 Framework 來開發，在處理 CSS Style 時，一般更傾向透過 `class` 來包含更多的樣式與狀態，而非透過 `style` 一一將樣式寫在 HTML 之中，不僅繁瑣也缺乏複用性 ; 同理，React 也可以透過動態加入 `class` 來更好的處理這種情況

首先，我們在 .css 檔案中加入特殊樣式

```css
.form-control.invalid label {
  color: red;
}

.form-control.invalid input {
  border-color: red;
  background: #ffd7d7;
}
```

接著就可以透過相同參數判斷是否顯示這些 `class`，在不符合的條件下，僅需回傳空字串即可

```js
return (
  <form onSubmit={formSubmitHandler}>
    <div className={`form-control ${!isValid ? 'invalid' : ''}`}>   // 條件判斷
      <label>Course Goal</label>
      <input type="text" onChange={goalInputChangeHandler} />
    </div>
    <Button type="submit">Add Goal</Button>
  </form>
);
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[Github](https://github.com/Jerry-Yeh/react-complete-guide/commit/d1a9c9979618b9f30b7e8a19c956a9b6de0f384a)
