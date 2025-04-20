---
title: Introducing Styled Components
abbrlink: 60289
date: 2022-06-12 16:35:59
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

到目前為止，在我們建立 Component 時，都會再額外建立一個同名的 css 檔案作為 Comoponent Style，但在 React 中，.css 一旦被任意 Component 導入，它就會成為 global style，而非只有導入的 Component 可以使用 (Scoped)，這在小專案也許不會是什麼問題，但當專案逐漸擴大、參與協作的人逐漸增加，就難免會發生 class 命名上重複的問題，進一步導致彼此的 CSS 互相覆蓋，這章節將介紹一個 React Plugin - [styled-components](https://styled-components.com/) 來解決這個問題

<!-- more -->

首先，不免俗的當然是先透過 npm 來安裝這個 plugin

```cmd
npm install --save styled-components
```

安裝完成後，在目標 Component 導入這個套件，這個 `styled` object 可以直接取代原先的 Component Function，而且這個 `styled` object 包含所有 HTML tag，每個 HTML tag 都是 function，接著在後方透過 [ES6 Template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#tagged_templates) 放入所有原先寫在 .css 裡面的 class style，但這裡需要稍微修改的是，可以不必加上 class name，只需要寫入 css attribute 即可，而如果是 Element state，則改成 `&` 來取代 

```js
import styled from 'styled-components'; 

const Button = styled.button`
  font: inherit;
  padding: 0.5rem 1.5rem;
  border: 1px solid #8b005d;
  color: white;
  background: #8b005d;
  box-shadow: 0 0 4px rgba(0, 0, 0, 0.26);
  cursor: pointer;

  &:focus {
    outline: none;
  }

  &:hover,
  &:active {
    background: #ac0e77;
    border-color: #ac0e77;
    box-shadow: 0 0 8px rgba(0, 0, 0, 0.26);
  }
`;
```

完成後會發現頁面和原先呈現相同的效果，打開 devtools Element 指向這個 HTML tag，styled-components 會幫我們產生 unique class name，如此一來，就不會有 global style 的問題了

![](unique-class-name.png)

## Dynamic Props

根據上方的範例，也許我們會有些疑問，因為這個範例太單純了，甚至不需要 `props` 其他條件來改變 `style`，那麼如果是更複雜的 Component 也能透過 styled-components 來改寫嗎 ? 答案是當然可以

以一個更複雜的 Component 為例，我們來改寫 `form` 內的 `div`，在改寫前其程式碼如下:

```js
// JS
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
      <div className={`form-control ${!isValid ? 'invalid' : ''}`}>
        <label>Course Goal</label>
        <input type="text" onChange={goalInputChangeHandler} />
      </div>
      <Button type="submit">Add Goal</Button>
    </form>
  );
};

```
```css
/* CSS */
.form-control {
  margin: 0.5rem 0;
}

.form-control label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
}

.form-control input {
  display: block;
  width: 100%;
  border: 1px solid #ccc;
  font: inherit;
  line-height: 1.5rem;
  padding: 0 0.25rem;
}

.form-control input:focus {
  outline: none;
  background: #fad0ec;
  border-color: #8b005d;
}

.form-control.invalid label {
  color: red;
}

.form-control.invalid input {
  border-color: red;
  background: #ffd7d7;
}
```

接著透過 styled-component 我們可以將 CSS 導入，並產生 `div` Component，同樣的將 class name 刪除，僅留下 css attribute，如果有 nested class、HTML tag or element state，則同樣透過 `&` 取代，如下:

```js
const FormControl = styled.div`
  margin: 0.5rem 0;

  & label {
    font-weight: bold;
    display: block;
    margin-bottom: 0.5rem;
  }

  & input {
    display: block;
    width: 100%;
    border: 1px solid #ccc;
    font: inherit;
    line-height: 1.5rem;
    padding: 0 0.25rem;
  }

  & input:focus {
    outline: none;
    background: #fad0ec;
    border-color: #8b005d;
  }

  &.invalid label {
    color: red;
  }

  &.invalid input {
    border-color: red;
    background: #ffd7d7;
  }
`;

const CourseInput = (props) => {
  ...
  return (
    <form onSubmit={formSubmitHandler}>
      <FormControl>
        <label>Course Goal</label>
        <input type="text" onChange={goalInputChangeHandler} />
      </FormControl>
      <Button type="submit">Add Goal</Button>
    </form>
  );
};
```

到這裡會發現，點選欄位並沒有顯示對應的 style 效果，這是當然的，因為我們還沒有補上透過 `isValid` 加以判斷 ; 透過 styled-components 產生的 Component 同樣可以帶入其他 `className` 來達到判斷的目的

```js
const CourseInput = (props) => {
  ...
  return (
    <form onSubmit={formSubmitHandler}>
      <FormControl className={!isValid && 'invalid'}>
        <label>Course Goal</label>
        <input type="text" onChange={goalInputChangeHandler} />
      </FormControl>
      <Button type="submit">Add Goal</Button>
    </form>
  );
};
```

但這裡有一個更進階的用法是，`props` 甚至可以直接傳入透過 styled-components 產生的 Components 內，直接將判斷寫在 CSS attribute 內，同樣可以將 props name 寫在 JSX 上，並在 Template literals 內透過 `${}` 寫入 callback function，其參數及是 `props` 

```js
const FormControl = styled.div`
  margin: 0.5rem 0;

  & label {
    font-weight: bold;
    display: block;
    margin-bottom: 0.5rem;
    color: ${props => props.invalid ? 'red' : ''}
  }

  & input {
    display: block;
    width: 100%;
    border: 1px solid ${props => props.invalid ? 'red' : '#ccc'};
    background: ${props => props.invalid ? '#ffd7d7' : 'transparent'};
    font: inherit;
    line-height: 1.5rem;
    padding: 0 0.25rem;
  }

  & input:focus {
    outline: none;
    background: #fad0ec;
    border-color: #8b005d;
  }
`;

const CourseInput = (props) => {
  ...
  return (
    <form onSubmit={formSubmitHandler}>
      <FormControl invalid={!isValid}>
        <label>Course Goal</label>
        <input type="text" onChange={goalInputChangeHandler} />
      </FormControl>
      <Button type="submit">Add Goal</Button>
    </form>
  );
};
```

第一次見到這種寫法真的會有點大開眼界，畢竟 HTML & CSS 都可以直接寫在 JS 內，並且確實解決了 global style 的問題

## Media Queries

在開發 Web 專案時，時常會需要配合各種裝置解析度做出對應的樣式調整，在 CSS 中，我們可以透過 media queries `@media` 來建立各種解析度範圍，而在 styled-components 中，這個用法完全可以直接應用，不需要做其他多餘的改寫

```js
const Button = styled.button`
  width: 100%;
  font: inherit;
  padding: 0.5rem 1.5rem;
  border: 1px solid #8b005d;
  color: white;
  background: #8b005d;
  box-shadow: 0 0 4px rgba(0, 0, 0, 0.26);
  cursor: pointer;

  @media (min-width: 768px) {
    width: auto;
  }
`;
```

## VSCode Extension

如果是 VSCode 的開發者，在使用 styled-components 這個 plugin 時，可以額外安裝 [vscode-styled-components](https://marketplace.visualstudio.com/items?itemName=styled-components.vscode-styled-components) 這個 extension，它不僅提供了 CSS emmet，還加上了 Syntax highlight，大大提高了開發效率。

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[MDN Template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#tagged_templates)
[Styled Components](https://styled-components.com/)
<!-- [Github](https://github.com/Jerry-Yeh/react-complete-guide/commit/08898606159d04d4eedc5b8c808679a73cf9e4b0) -->
