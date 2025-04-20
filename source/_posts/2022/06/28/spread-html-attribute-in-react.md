---
title: Spread HTML attribute in React
abbrlink: 12461
date: 2022-06-28 23:14:53
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

在使用 HTML form 表單相關 elemnt 時，常會透過許多內建的 attribute 來定義格式，在 React 中，如果將這些 element 封裝成 component，可以透過 props 的方式彈性改變 attribute 

<!-- more -->

一個封裝的 input component 如下，我們希望他可以彈性的改變 attribute 條件，注意這裡的 input prop 使用了兩層大括號 {}，原因是第一層是為了使用 string 以外的資料，第二層才是欲帶入 object data

```js
// Form.js
import Input from './Input.js';

const Form = () => {
  return (
    <form>
      <Input label="Amount" input={{
        id: "amount",
        type: "number",
        min: "1",
        max: "5",
        step: "1",
        defaultValue: "1"
      }}/>
    </form>
  );
};
```

接著在封裝的 input component 內，label 上的 `htmlFor` 其實就是原生的 `for`，用來讓 label 綁定對應的 element，而 input 透過 ES6 spread 直接展開 props.input object，就可以使 key 直接成為 element attribute，value 自然就是 attribute value。 

```js
// Input.js
const Input = props => {
  return (
    <label htmlFor={props.input.id}>{props.label}</label>
    <input {...props.input}></input>
  );
};

```

不只是 input，這樣的方法適用於各種 HTML element，透過這種方式就可以使 component 的應用上更加彈性

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[GitHub](https://github.com/Jerry-Yeh/React-The-Complete-Guide-Section9/commit/13bb593e48b153251864042e40097d06b691a3ed)
