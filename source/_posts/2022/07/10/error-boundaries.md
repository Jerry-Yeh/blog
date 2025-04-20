---
title: Error Boundaries
abbrlink: 12258
date: 2022-07-10 17:42:59
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
  - Class-based
categories: React
thumbnail:
banner:
description:
---

有時候在操作網頁時，難免會遇到一些錯誤，這裡的錯誤指的不是程式碼中的 bug，而是可能會有搜尋沒有結果、網路連線或資料取得上出了問題，這時候就會需要有對應的回饋來讓使用者有一個比較完整的使用體驗。

<!-- more -->

在原生的 JavaScript 中，可以使用 `try catch` 來 handle 錯誤訊息，但在 React 中，這只適用於單一元件內，如果需要將錯誤訊息在父子元件中傳遞，就會需要 Error Boundary 來處理，而在 Class-based 中，有一個 Lifecycle - `componentDidCatch()` 可以處理這個問題 ; 但這裡要注意的是，在 functional component 中沒有完全對應的方法。

首先，新增一個錯誤情境在子元件中，判斷資料在特定條件下，會拋出錯誤訊息

```js
// ChildrencomponentDidUpdate() {
  if (this.state.users.length === 0) {
    throw new Error('No users provided!');
  }
}
```

接著，會建立一個 Class-based component 來 handle 這些錯誤，它的基礎結構其實就和一般的 Class-based component 完全相同，並有一個 State 判斷是否有錯誤，如果有就回傳錯誤提示，如果沒有就照常回傳 JSX

```js
class ErrorBoundary extends Component {
  constructor() {
    super();
    this.state = {
      hasError: false
    };
  }

  render() {
    if (this.state.hasError) {
      return <p>Something went wrong!</p>
    }

    return this.props.children;
  }
}
```

比較特別的是，會有一個 Lifecycle - `componentDidCatch()` 來取得錯誤訊息，如果有錯誤，就更改判斷錯誤的 State

```js
class ErrorBoundary extends Component {
  constructor() {
    super();
    this.state = {
      hasError: false
    };
  }

  componentDidCatch(error) {
    console.log(error);
    this.setState({hasError: true});
  }

  render() {
    if (this.state.hasError) {
      return <p>Something went wrong!</p>
    }

    return this.props.children;
  }
}
```

使用方式其實也很單純，就是把 ErrorBoundary 直接包住要 handle 的 component 即可，這麼一來，`componentDidCatch()` 所取得的 error 就會是其包覆的 comopnent 所傳遞的錯誤訊息

```js
// Parent
import Users from "./Users";
import ErrorBoundary from "./ErrorBoundary";

class UserFinder extends Component {
  ...
  render() {
    return (
      <ErrorBoundary>
        <Users />
      </ErrorBoundary>
    );
  }
}
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
