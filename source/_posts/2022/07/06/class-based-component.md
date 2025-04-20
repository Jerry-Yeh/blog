---
title: Class-based Component
abbrlink: 10203
date: 2022-07-06 22:04:27
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
  - Class-based
categories: React
thumbnail:
banner:
description:
---

目前為止 React 所有章節的內容，都是由 function 來建立 component，function 中有參數 props 來傳遞父子元件的資料，並回傳 JSX 渲染頁面 ; 而在 React 中，透過 function 建立 component 其實不是唯一的方式，React 還可以透過 class 來建立 component，但這完全是選擇性的，即便沒有學會這種方式，還是完全可以開發 React ; 這種方式可能會在一些比較早期的 React 專案或 third-party libary 中見到。  

<!-- more -->

## What & Why

在 React 16.8 以前，只能透過 Class-based component 來處理 State 資料，而從 React 16.8 開始，新增了許多過去章節所提到的 React hooks，這使的我們可以完全透過 Functional component 來開發 React，而這裡要特別注意的是，這些 React hooks 無法在 Class-based component 中使用 ; 想當然爾，會有其他的方法來取代諸如 `useEffect()`、`useState()` 等 React hooks 所解決的問題。

## Adding a First Class-based Component

一個最基本的 Funcitonal component 範例如下:

```js
const User = props => {
  return <li className={classes.user}>{props.name}</li>;
};
```

如果要將其透過 Class-based 改寫，首先，其實和原生的 JavaScript 語法一樣，透過 `class` 建立，只是這裡比較特別的是，不會透過 `construcor` 建立初始值，而是透過一個 render function 回傳 JSX 

```js
class User {
  render() {
    return <li className={classes.user}>{props.name}</li>;
  }
};
```

接著可能會問到，那麼該如何取得 `props` 呢 ? 不同於 functional 直接在第一個參數取得，這裡需要透過 react import Component，再透過原生 JavaScript 語法 `extends` 來繼承，這麼一來就可以透過 `this.props` 來取得 `props` 了

```js
import { Component } from 'react';

class User extends Component {
  render() {
    return <li className={classes.user}>{this.props.name}</li>;
  }
};
```

### 共存

還有一個很重要的觀念是，在 React 16.8 以前，只有 Class-based component 可以處理 State，這也表示這兩種建立 React component 的方式是可以在同一個專案中共存的! 

當然一般如果是從頭開始一個專案，還是會比較建議從一而終選擇一種方式開發，而從上面簡單的範例中就可以看出，Functional component 明顯更簡潔一點，這也就是它在新的 React 中更受歡迎的原因了。

## Work with State and Event

接著看到一個稍微複雜一點的 component，看完可能會更堅信 Functional 是更簡潔的方式 ; 這個 component 包含了 State & Event，先來看到原始 Functional 的版本，如下:

```js
import { useState } from "react";
import User from "./User";

const DUMMY_USERS = [
  { id: "u1", name: "Max" },
  { id: "u2", name: "Manuel" },
  { id: "u3", name: "Julie" },
];

const Users = () => {
  const [showUsers, setShowUsers] = useState(true);

  const toggleUsersHandler = () => {
    setShowUsers((curState) => !curState);
  };

  const usersList = (
    <ul>
      {DUMMY_USERS.map((user) => (
        <User key={user.id} name={user.name} />
      ))}
    </ul>
  );

  return (
    <div className={classes.users}>
      <button onClick={toggleUsersHandler}>
        {showUsers ? 'Hide' : 'Show'} Users
      </button>
      {showUsers && usersList}
    </div>
  );
};

export default Users;
```

透過 Class 來改寫的話，首先，一樣透過 extends Component 來建立 component，並透過 render function 回傳 JSX

```js
class Users extends Component {
  render() {
    const usersList = (
      <ul>
        {DUMMY_USERS.map((user) => (
          <User key={user.id} name={user.name} />
        ))}
      </ul>
    );

    return (
      <div className={classes.users}>
        <button onClick={toggleUsersHandler}>
          {showUsers ? 'Hide' : 'Show'} Users
        </button>
        {showUsers && usersList}
      </div>
    );
  }
}
```

### State

在 Class-based 中，並沒有 React hooks 來建立 State，如果要建立 State，必須透過原生語法 `construcor` 建立，而且名稱規定就是 `this.state`，值只能是一個 object，其中的每一個 key 就是一個 State 的值，value 就可以是任意型別了; 如果要使用 State，就必須在前方加上 `this.state` 指定目標 key

```js
class Users extends Component {
  constructor() {
    this.state = {
      showUsers: true,
      more: []
    };
  }
  
  render() {
    const usersList = ...;

    return (
      <div className={classes.users}>
        <button onClick={toggleUsersHandler}>
          {this.state.showUsers ? 'Hide' : 'Show'} Users
        </button>
        {this.state.showUsers && usersList}
      </div>
    );
  }
}
```

### Event

如果要建立 Event，可以直接在 Class 中建立 function，改變 State 則需透過呼叫 `this.setState`，這個方法就是來自於繼承 Component，其中的值如果單純以新的值覆蓋，就帶入一個 object，其中只要帶入想更改的 key，其餘沒有帶入的 key 則會以 merge 的方式維持不變，最後一樣透過 `this.toggleUsersHandler` 來呼叫
```js
class Users extends Component {
  constructor() {
    this.state = {
      showUsers: true,
      more: []
    };
  }

  toggleUsersHandler() {
    this.setState({
      showUsers: false
    });
  }
  
  render() {
    const usersList = ...;

    return (
      <div className={classes.users}>
        <button onClick={this.toggleUsersHandler}>
          {this.state.showUsers ? 'Hide' : 'Show'} Users
        </button>
        {this.state.showUsers && usersList}
      </div>
    );
  }
}
```

但如果需要依賴舊的值來產生新的值，則和 `useState()` 一樣帶入 function，第一個參數就是舊的值

```js
toggleUsersHandler() {
  this.setState((curState) => {
    return {showUsers: !curState.showUsers};
  });
}
```

### this

到目前為止看似一切正常，但當我們按下按鈕呼叫這個 Event 後，會出現以下 error log

![](this.png)

原因其實不在於 React，而在於原生 JavaScript `this` 的關係，為了避免呼叫 function 時，`this` 沒有指向 class 的問題，在呼叫 function 時可以透過 `bind(this)` 即時將 `this` 指向 class

```js
  render() {
    const usersList = ...;

    return (
      <div className={classes.users}>
        <button onClick={this.toggleUsersHandler.bind(this)}>
          {this.state.showUsers ? 'Hide' : 'Show'} Users
        </button>
        {this.state.showUsers && usersList}
      </div>
    );
  }
```

### super()

以為結束了嗎 ? 修正上一個 `this` 的 error log 後，發現又出現另外一個 error log，這個問題一樣也是原生 JavaScript 的關係，在 React Class-based component 中，我們透過 `extends Component` 繼承了許多 React 的方法，所以在使用前，需要在 `constructor` 中呼叫 `super()`

![](super.png)

```js
constructor() {
  super();
  this.state = {
    showUsers: true,
    more: []
  };
}
```


## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
