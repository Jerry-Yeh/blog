---
title: Class-based component & Context
abbrlink: 62283
date: 2022-07-07 23:13:02
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
  - Class-based
categories: React
thumbnail:
banner:
description:
---

這章節將會介紹如何在 Class-based component 中使用 Context

<!-- more -->

因為 Class-based & Funcitonal component 可以在同一個專案中共存，所以在 Context 的建立上完全相同，只有在使用的方式不一樣，一個簡單的 Context 如下:

```js
// users-context.js
const UsersContext = React.createContext({
  users: []
});

// App.js
const DUMMY_USERS = [
  { id: "u1", name: "Max" },
  { id: "u2", name: "Manuel" },
  { id: "u3", name: "Julie" },
];

function App() {
  const usersContext = {
    users: DUMMY_USERS
  };

  return (
    <UsersContext.Provider value={usersContext}>
      <UserFinder />
    </UsersContext.Provider>
  );
}
```

而在使用上，其實沒有和 `Context.Comsumer` 完全對等的使用方式，而是要透過 `static` 定義 context，而且每個 Class-based component 只能有一組 context，如果需要多組 context 就需要另外找其他方式來實現 ，雖然大部分情況我們不會需要在一個 comopnent 內使用到多組 context

```js
import UsersContext from '...';

class UserFinder extends Component {
  static contextType = UsersContext;
} 
```

使用上則可以直接透過 `this.context` 指向對應的資料

```js
class UserFinder extends Comopnent {
  componentDidMount() {
    // Send http reqeust...
    this.setState({filteredUsers: this.context.users});
  }
}
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
