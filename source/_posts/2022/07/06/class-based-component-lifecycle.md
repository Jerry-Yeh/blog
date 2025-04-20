---
title: Class-based Component Lifecycle
abbrlink: 50342
date: 2022-07-06 23:49:54
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
  - Class-based
categories: React
thumbnail:
banner:
description:
---

因為在 Class-based 中無法使用任何 React hook，所以勢必有其他方式來取代 hook 所解決的問題，接下來接介紹取代 `useEffect()` 的方式 - Lifecycle

<!-- more -->

## Lifecycle

在 Class-based 中，取代 `useEffect()` 的方式就稱為 Lifecycle，其中有 3 組 function 分別表示 3 種 `useEffect()` 觸發的時機

- `componentDidMount()`: component 生成時觸發，等同於 `useEffect(..., [])` 在 dependency array 為空的情況
- `componentDidUpdate`: component dependency 發生異動時觸發，等同於 `useEffect(..., [someValue])` dependency array 中的值改變時機
- `componentWillUnmount`: 在 component 從 DOM 移除時觸發，等同於 `useEffect()` cleanup function `useEffect(() => {return () => {...}, [])`  

## Lifecycle Methods in Action

直接透過一個範例來了解 Class-based Lifecycle 可能會更加清楚，一個包含 `useEffect()` 的範例如下:

```js
const UserFinder = () => {
  const [filteredUsers, setFilteredUsers] = useState(DUMMY_USERS);
  const [searchTerm, setSearchTerm] = useState('');

  useEffect(() => {
    setFilteredUsers(
      DUMMY_USERS.filter((user) => user.name.includes(searchTerm))
    );
  }, [searchTerm]);

  const searchChangeHandler = (event) => {
    setSearchTerm(event.target.value);
  };

  return (
    <Fragment>
      <input type='search' onChange={searchChangeHandler} />
      <Users users={filteredUsers} />
    </Fragment>
  );
};
```

`useEffect()` 以外的改寫在上一章節中都提到過了，所有 State 會由 `this.state` object 在 `constructor` 內定義，event 可以直接在 `class` 內定義，但在 JSX 上呼叫時需透過 `bind(this)` 使其指向 class 本身

```js
class UserFinder extends Component {
  constructor() {
    super();
    this.state = {
      filteredUsers: DUMMY_USERS,
      searchTerm: ''
    };
  }

  searchChangeHandler(event) {
    this.setState({
      searchTerm: event.target.value
    });
  }

  render() {
    return (
      <Fragment>
        <input type="search" onChange={this.searchChangeHandler.bind(this)} />
        <Users users={this.state.filteredUsers} />
      </Fragment>
    );
  }
}
```

### componentDidUpdate

而 `useEffect()` 就需要透過 Lifecycle 來改寫了，因為這組 `useEffect()` 有一個 dependency searchTerm，所以使用的 Lifecycle 是 `componentDidUpdate`，其中兩個參數如下

- prevProps: Props snapshot
- prevState: State snpashot

如果需要透過 State 作為 dependency，就會需要判斷 State 前後的值是否相等，否則當 State 異動時，就會造成 infinite loop 的情況

```js
componentDidUpdate(prevProps, prevState) {
  if (prevState.searchTerm !== this.state.searchTerm) {
    this.setState({
      filteredUsers: DUMMY_USERS.filter((user) =>
        user.name.includes(this.state.searchTerm)
      ),
    });
  }
}
```

### componentDidMount

而 `componentDidMount()` 則是在 component 生成時，只會執行一次，使用情境常會在許多一次性的行為，如http 驗證登入

```js
componentDidMount() {
  // Send http request
  this.setState({
    filteredUsers: DUMMY_USERS
  });
}
```

### componentWillUnmount

則是在 component 被移除時觸發，如上範例因為 DUMMY_USERS 內有三筆資料，透過 `map()` 在 JSX 中生成 3 組 User component，如果切換移除 User component， `componentWillUnmount` 這個 Lifecycle 也就會分別執行 3 次

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
