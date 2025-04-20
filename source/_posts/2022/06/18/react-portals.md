---
title: React Portals
abbrlink: 21072
date: 2022-06-18 11:27:40
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

這章節將介紹另一個 React 的功能 - Portals，他可以使我們的程式碼更符合操作邏輯和語意，不只是技術上可行而已 

<!-- more -->

一個具有 modal 的範例如下:

```js
return (
  <React.Fragment>
    <MyModal />
    <MyInputForm />
  </React.Fragment>
);
```

透過這樣的寫法，在渲染為實際的 DOM 後，結構上會像是以下這樣的結構:

```js
return (
  <section>
    <h2>Some other content...</h2>
    <div class="my-modal">
      <h2>A Modal Title!</h2>
    </div>
    <form>
      <label>Username</label>
      <input type="test"/>
    </form>
  </section>
);
```

這樣的結構在技術上完全可行，但就頁面顯示來說，modal 應該是一個滿版覆蓋 (overlay) 的 component，而不是放置於 component 內，所以合理的結構應該如下:

```js
return (
  <div class="my-modal">
    <h2>A Modal Title!</h2>
  </div>
  <section>
    <h2>Some other content...</h2>
    <form>
      <label>Username</label>
      <input type="test"/>
    </form>
  </section>
);
```

## Working with Portals

為了解決讓 DOM 渲染在正確位置的問題，react-dom 提供了一個方法來達到這個目的，注意這裡說的是 react-dom 而不是 react 核心，react-dom 是被額外延伸出來負責處理 DOM 的 plugin，還有許多 plugin 如 react-native 則專門處理打包成桌面應用程式。

首先，我們在 index.html 中新增兩個 element 分別用來放置 backdrop & overlay

```html
<!-- index.html -->
<html>
<body>
  <div id="backdrop-root"></div>
  <div id="overlay-root"></div>
  <div id="root"></div>
</body>
</html>
```

### ReactDOM.createPortal()

接著便可在原先的 ErrorModal 中導入 `ReactDOM.createPortal()` 方法，藉由這個方法便可以將 JSX 指定渲染到上述於 index.html 中所新增的位置，而為了個別指定 backdrop & overlay，可以將這兩組 JSX 分別區分為兩個 Component

```js
const BackDrop = (props) => {
  return <div className={classes.backdrop} onClick={props.onConfirm} />;
};

const ModalOverlay = (props) => {
  return (
    <Card className={classes.modal}>
      <header className={classes.header}>
        <h2>{props.title}</h2>
      </header>
      <div className={classes.content}>{props.message}</div>
      <footer className={classes.actions}>
        <Button onClick={props.onConfirm}>Okay</Button>
      </footer>
    </Card>
  );
};
```

`ReactDOM.createPortal()` 這個方法需調入兩個參數，方別為:

- Component: 欲渲染的 component
- Pointer: 指定渲染的位置，可以使用原生方法 `ducment.getElementById()` 來指定

```js
const ErrorModal = (props) => {
  return (
    <React.Fragment>
      {ReactDOM.createPortal(
        <BackDrop onConfirm={props.onConfirm} />,
        document.getElementById("backdrop-root")
      )}
      {ReactDOM.createPortal(
        <ModalOverlay
          title={props.title}
          message={props.message}
          onConfirm={props.onConfirm}
        />,
        document.getElementById("overlay-root")
      )}
    </React.Fragment>
  );
};
```

這麼一來在觸發 modal 啟動時，就會發現以上 component 被渲染到指定位置了

![](rendered.png)

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
