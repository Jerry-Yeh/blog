---
title: React Strict Mode
categories: React
abbrlink: 65126
date: 2022-10-15 11:45:13
tags:
thumbnail:
banner:
description:
---

<!-- @format -->

在 React 18 中，如果開啟 Strict Mode，每當 component mounts 後，就會立即 unmount 再 remount 一次，這麼做的目的是提醒非必要的行為有被正確的清除，不過也不用擔心效能問題，因為這個行為只會發生在 development 模式下，所以非常建議在開發中啟動 Strict Mode，而非關閉它

<!-- more -->

## Turn on Strict Mode

在 React 18 的專案中要開啟 Strict Mode 也非常容易，只要在進入點 index.tsx 透過 react 取得 `StrictMode`，並將其包覆根元件，這麼一來整個專案的元件就會啟動 Strict Mode 了

```ts
// index.tsx
import { StrictMode } from "react";
import ReactDOM from "react-dom/client";
import { RouterProvider } from "react-router-dom";

import router from "router";

const root = ReactDOM.createRoot(
  document.getElementById("root") as HTMLElement
);
root.render(
  <StrictMode>
    <RouterProvider router={router} />
  </StrictMode>
);
```

## How to handle the Effect firing twice in development?

開啟 Strict Mode 後，useEffect 中的行為就會在開始時，被連續觸發兩次，而我們要思考的不是怎麼讓它只觸發一次，而是怎麼解決在 remount 後所發生的事情。

在大部分的情況中，可以透過正確的使用 cleanup 來解決這個問題，以下是大部分開發中會出現的情境:

### Controlling non-React widgets

如果是一個與 React 沒有綁定 UI 元件，寫法其實就和一般寫法相同;舉個例子，這裡我們透過 `setZoomLevel` method 新增了一個 map component，因為直接使用 `zoomLevel` state，所以並不需要特別處理，反正在 production 也只會執行一次

```ts
useEffect(() => {
  const map = mapRef.current;
  map.setZoomLevel(zoomLevel);
}, [zoomLevel]);
```

但有些 API 即使在 development 可能也不希望呼叫兩次，例如一個 dialog 元件顯示，這種情況就可以在 cleanup 來立即關閉它，這麼一來顯示上就會如 production 只顯示一次

```ts
useEffect(() => {
  const dialog = dialogRef.current;
  dialog.showModal();
  return () => dialog.close(); // close dialog
}, []);
```

### Subscribing to events

如果是透過事件監聽 `addEventListener()` 註冊一個事件，就可以在 cleanup 使用 `removeEventListener()` 來取消監聽

```ts
useEffect(() => {
  function handleScroll(e) {
    console.log(e.clientX, e.clientY);
  }
  window.addEventListener("scroll", handleScroll);
  return () => window.removeEventListener("scroll", handleScroll); // remove listener
}, []);
```

### Triggering animations

如果是透過調整 css 參數來影響畫面呈現的情況，就可以在 cleanup 將參數條回預設值，例如 opacity 由 0 -> 1 的變化

```ts
useEffect(() => {
  const node = ref.current;
  node.style.opacity = 1; // Trigger the animation
  return () => {
    node.style.opacity = 0; // Reset to the initial value
  };
}, []);
```

### Fetching data

而如果是非同步的網路請求，就無法在送出後還要求撤回，因此需要透過一個變數來阻止重複更新資料，並在第一次呼叫後，於 cleanup 更改狀態，這麼一來更新資料的動作就只會執行一次，這麼做的目的是測試避免有任何相關聯的行為影響到資料

```ts
useEffect(() => {
  let ignore = false;

  async function startFetching() {
    const json = await fetchTodos(userId);
    if (!ignore) {
      setTodos(json);
    }
  }

  startFetching();

  return () => {
    ignore = true;
  };
}, [userId]);
```

在 development 環境可以透過 Devtool Network 發現，網路請求確實執行了兩次，但同樣的，在 production 環境只會被執行一次

## Sending analytics

有些行為執行一次或兩次在使用者眼中並沒有差別，例如畫面的切換，在這種情況下就建議維持原有寫法即可

```ts
useEffect(() => {
  logVisit(url); // Sends a POST request
}, [url]);
```

## Not an Effect: Initializing the application

而有些邏輯只會在整個應用程式啟動時執行一次，這種功能就可以將其獨立於 component 之外，不隨著 component 更新而觸發

```ts
if (typeof window !== "undefined") {
  // Check if we're running in the browser.
  checkAuthToken();
  loadDataFromLocalStorage();
}

function App() {
  // ...
}
```

## Not an Effect: Buying a product

而有些行為同樣也不希望被執行兩次，例如送出購買的需求，如果按返回鈕就被重複執行，這種情況想必不太樂見

```ts
useEffect(() => {
  // 🔴 Wrong: This Effect fires twice in development, exposing a problem in the code.
  fetch("/api/buy", { method: "POST" });
}, []);
```

所以也不該寫在 Effect 之中，而是透過實際使用者主動的操作行為來觸發

```ts
function handleClick() {
  // ✅ Buying is an event because it is caused by a particular interaction.
  fetch("/api/buy", { method: "POST" });
}
```

## Recap

- 有別於事件，Effects 觸發於 component render 的時機，而不是特定的使用者交互作用
- Effects 提供外部的系統(如: 第三方 API 或網路請求行為) 與 component 同步
- 預設的情況下，Effect 會在每次的 component render 都被執行
- 你無法自行決定 dependencies，它來自於 Effect 中所引用的參數
- 空的 dependency array `[]` 代表 component mounting 的行為
- 如果為 React 專案加上 Strict Mode，就會在 development mount component 的階段執行兩次 Effects 中的行為以利壓力測試
- 如果 Effect 行為終止於 remounting 階段，就需要使用 cleanup function
- React 會在下一次 Effect 執行之前，呼叫 cleanup function，也就是 unmount 階段

## 資料參考

[How to handle data fetching happening twice?](https://beta.reactjs.org/learn/synchronizing-with-effects#how-to-handle-the-effect-firing-twice-in-development)
[[stackoverflow] useEffect is running twice on mount in React](https://stackoverflow.com/questions/72238175/useeffect-is-running-twice-on-mount-in-react)
