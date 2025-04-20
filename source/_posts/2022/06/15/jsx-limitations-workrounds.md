---
title: JSX Limitations & Workrounds
abbrlink: 47101
date: 2022-06-15 23:26:07
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

這章節將來討論 JSX 的限制與解決方法

<!-- more -->

在過往的章節中，反覆提到的一個概念就是 component function 所 return 的 JSX 只有有一個 element，原因是在前面也有提到過，當我們 return 一組 element 時，React 就會執行 `React.createElement()` 來生成實際的 DOM element，而在 JavaScript 中，function return 本來就只能回傳一個值，無論是基本型別或物件也好，都只能是一個，所以在 component 內有多個 element 時，解決方法如下:

## 方法一: Parent Element

最直接的方法就是透過一個 parent element 包住所有 element， 常見的做法就是直接使用一組 `div`，

```js
const Component = () => {
  return (
    <div>
      <div></div>
      ...
      <div></div>
    </div>
  );
};
```

## 方法二: Array

前面的章節其實也提供了另一種方式，那就是 return array，在 JSX 中，element list 是可以直接被解析的，如同先前的範例透過 `Array.map()` 渲染出 data list item

```js
const Component = props => {
  return (
    <ul>
      {props.listData.map(item => {
        return (
          <li>...</li>
        );
      })}
    </ul>
  );
};
```

透過這種方式也可以直接使用 array [] 取代 parent element，並透過逗號 , 隔開所有 element

```js
const Component = () => {
  return (
    [
      <div></div>,
      ...
      <div></div>,
    ]
  );
};
```

但按照先前的經驗，在 React 中，透過 array 產生 element 就需要賦予每個 item 一組 unique id，這裡當然也不例外

```js
const Component = () => {
  return (
    [
      <div key="e1"></div>,
      ...
      <div key="en"></div>,
    ]
  );
};
```

但很顯然的，既然還要主動賦予一組 unique key，那還不然乾脆就加上 parent element 就解決了，雖然這麼做完全可行，但也間接產生了其他問題是，有些 CSS style 是直接指定 child element，多餘的多層非必要 parent element 可能會導致無法指向預期的目標，再加上這些非必要的 parent element 也會增加瀏覽器和 React 在處理上的負擔，進而影響網頁效能。

## 方法三: Wrapper Component

縱關上述的兩個方式，似乎都還不是很理想，因此 React 提供了另一個方式 - Wrapper Component，顧名思義就是建立一個名為 Wrrapper 的 Component，並在 Component 中直接回傳 `props.chilren`，再使用這個 Component 取代原先的 parent element (div)，這麼一來就可以在符合 return 一個 element 的原則下不應加額外 element 的目的了

```js
// Wrapper.js
const Wrapper = props => {
  return (
    props.children
  );
};

// Else Component
import Wrapper from '@/components/Helpers/Wrapper';

const component = () => {
  return (
    <Wrapper>
      ...
    </Wrapper>
  );
};
```

## React Fragments

因為這個方式完整的解決了這個問題，所以 React 甚至幫我們寫好了，透過以下 3 種方式就可以達到相同的目的

- React.Fragment: 直接取代自製 Wrapper 即可，這個方式適用於所有 React 專案環境

```js
const component = () => {
  return (
    <React.Fragment>
      ...
    </React.Fragment>
  );
};
```

- Fragment: 這個方法使用方式相同，使用上也更為簡潔，但並非支援於所有 React 專案，需視專案建立環境而訂

  ```js
  const component = () => {
    return (
      <Fragment>
        ...
      </Fragment>
    );
  };
  ```

- empty: 最後也是最簡潔的方式是，完全可以直接省略 element 中的文字，這種方式並非原生 HTML tag，而是 JSX 獨有的語法

  ```js
  const component = () => {
    return (
      <>
        ...
      </>
    );
  };
  ```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
