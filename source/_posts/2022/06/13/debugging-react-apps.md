---
title: Debugging React Apps
abbrlink: 54546
date: 2022-06-13 08:03:36
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

這章節將介紹透過 Chrome Debug 的方式

<!-- more -->

首先，我們先讓程式碼故意出錯，在新增於 list 的 item ID 上，使其固定，重複的 ID 便會使 React 出錯，這時候可以打開 Chrome / devtools / Sources，並找出該操作行為所指向的那行程式碼，點擊最前方的行數便會發現剛行被選取了

![](breakpoint.png)

接著在畫面上執行我們預期會發生的錯誤操作，便會發現在畫面上方出現 `Paused in debugger`，且畫面操被暫停於操作前，代表程式碼並沒有依序執行下去，接下來我們將透過點選 `Step into next function call` 按鈕，程式碼就會按照步驟一步一步跳轉

![](step-into-function-call.png)

透過這個方式，我們就可能間接或直接的找到程式碼中錯誤的位置，進而加以修正

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
