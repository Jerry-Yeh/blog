---
title: prop className
abbrlink: 12696
date: 2022-06-14 23:25:07
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

這章節將透過傳遞 class 達到擴充 Component class style 的目的

<!-- more -->

## props.className

在許多開發情境中，時常會將重複的樣式單獨拉出去為一個 Component 以供複用，一個外層 Component 的應用如下

```js
// Card.js
import {useState} from 'react';

import classes from './Card.module.css';

const Card = (props) => {
  return (
    <div className={`${classes.card}`}>{props.children}</div>
  );
};

// AddUser.js
import {useState} from 'react';

import classes from './AddUser.module.css';
import Card from '../UI/Card';

const AddUser = (props) => {
  return (
    <Card className={classes.input}>
      ...
    </Card>
  );
};
```

但如果只透過上方的寫法各自導入 CSS 會發現，AddUser 中的 element 並沒有實際取得 AddUser.module.css 中的 CSS Style，原因是 Card 並不是內建的 HTML element，而是我們自製的 component，AddUser 中的 element 會透過 `props.children` 傳進 Card 對應位置，所以 CSS Style (classes)也需要被傳入，而 classes 是一個 object，所以 prop name 並非一定得是 `className`，只是為了開發上統一，一般還是會沿用 `className` 作為 CSS Style

```js
// Card.js
import {useState} from 'react';

import classes from './Card.module.css';

const Card = (props) => {
  return (
    <div className={`${classes.card} ${props.className}`}>{props.children}</div>
  );
};
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
