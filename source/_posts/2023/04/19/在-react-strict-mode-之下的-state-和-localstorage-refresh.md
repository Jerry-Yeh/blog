---
title: 在 React strict mode 之下的 state 和 localStorage refresh
abbrlink: 60376
date: 2023-04-19 16:23:43
tags: Local Storage
  Strict mode
categories: React
thumbnail:
banner:
description:
---

<!-- @format -->

紀錄在 React strict mode 之下，透過 Local Storage 儲存 state 資料，並在 refresh 頁面後保留資料的方式

<!-- more -->

一般在 refresh 頁面後，component 的 state 資料就會被還原回初始值; 然而在某些情況，如: 第三方登入，會將頁面跳轉到第三方自家的認證頁面，並在成功後跳轉回 APP 頁面，這樣的行為就相當於 refresh 頁面，但有時候會希望不透過 API 將暫時資料儲存在 server，這時候最直覺的做法就是將資料儲存在 Local Storage。

一段簡易的使用者資料如下:

```jsx
const App = () => {
  const [user, setUser] = useState({ name: "" });

  return (
    <div>
      <button>Google 登入</button>
    </div>
  );
};
```

如果要與 Local Storage 的資料同步，就會在頁面載入時，取得 Local Storage 的資料，並在資料更新時同步更新 Local Storage 的資料，所以就可以透過 `useEffect` 執行這兩個動作

```jsx
const App = () => {
  const [user, setUser] = useState({ name: "" });

  // 取得 Local Storage 的資料
  useEffect(() => {
    setUser(JSON.parse(localStorage.getItem("user")));
  }, []);

  // 更新 Local Storage 的資料
  useEffect(() => {
    if (user) localStorage.setItem("user", JSON.stringify(user));
  }, [user]);

  return (
    <div>
      <button>Google 登入</button>
    </div>
  );
};
```

## Strict Mode

以上的作法在一般情況執行沒有問題，但如果在 React 18 strict mode 的環境下執行，就會發現資料並沒有被儲存到，但透過 `console.log` 印出資料就會發現，並不是資料沒有被更新到 Local Stroage，而是更新後資料又被初始值所覆蓋

```jsx
const App = () => {
  const [user, setUser] = useState({ name: "" });

  // 取得 Local Storage 的資料
  useEffect(() => {
    console.log("get", JSON.parse(localStorage.getItem("user")).name);
    setUser(JSON.parse(localStorage.getItem("user")));
  }, []);

  // 更新 Local Storage 的資料
  useEffect(() => {
    console.log("set", user.name);
    if (user) localStorage.setItem("user", JSON.stringify(user));
  }, [user]);

  return (
    <div>
      <button>Google 登入</button>
    </div>
  );
};
```

原因是在 strict mode 之下，component 會被 render 兩次，而 state 從無到初始資料的過程就會觸發更新 Local Storage 的動作，所以在取得新的資料後，又把初始資料更新回去; 而為解決這種情況，除了依賴其他第三方套件之外，可以判斷如果 Local Storage 有資料，就直接以它為初始資料

```jsx
const App = () => {
  // 判斷 Local Storage 是否有資料，如有擇以它為初始資料
  const [user, setUser] = useState(() =>
    localStorage.getItem("user")
      ? JSON.parse(localStorage.getItem("user"))
      : { name: "" }
  );

  // 更新 Local Storage 的資料
  useEffect(() => {
    console.log("set", user.name);
    if (user) localStorage.setItem("user", JSON.stringify(user));
  }, [user]);

  return (
    <div>
      <button>Google 登入</button>
    </div>
  );
};
```

## 資料參考

[Why is localStorage getting cleared whenever I refresh the page?](https://stackoverflow.com/questions/72222728/why-is-localstorage-getting-cleared-whenever-i-refresh-the-page)
[5 Methods to Persisting State Between Page Reloads in React](https://blog.bitsrc.io/5-methods-to-persisting-state-between-page-reloads-in-react-8fc9abd3fa2f)
