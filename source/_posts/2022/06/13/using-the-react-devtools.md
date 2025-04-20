---
title: Using the React Devtools
abbrlink: 64148
date: 2022-06-13 23:07:45
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

在前一章節中，我們介紹了如透過 Chrome devtools 來 Debug React 專案但其實那個方式並非僅限於 React，而是任何 Web 開發都適用，接下來將介紹一個完全為了 React 量身打造的 Chrome Plugin Debugger Tool - [React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)

<!-- more -->

就如同其他 Chrome Plugin 一樣，在 Chrome 擴充功能中搜尋即可快速安裝，安裝並啟動後，在打開 Chrome devtools 會發現多了兩個 tab Components & Profiler

![](chrome-plugin.png)

## Components

首先，切換到 Components 會發現當下頁面所有 Component 會依序從 Root Component 到 Child Componet 完整列出，且點選 Component 時還會在頁面上 highlight 對應的 comoponent，更包含詳細資訊如下:

- props: 傳入 component 的資料
- rendered by: parent component 路徑
- souce: 程式碼位置

![](components.png)

甚至可以在 devtools 中直接修改，畫面也會動態的隨之調整

![](updated.png)

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)
