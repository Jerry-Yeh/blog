---
title: 'Debug 神器: Debugger'
abbrlink: 20624
date: 2021-10-17 17:57:22
tags:
  - Chrome
  - Debug
categories: Browser
thumbnail:
banner:
description:
---

紀錄一下透過胡立大在 Lidemy 平台上 `先別急著寫 leetcode` 這堂課中所學到的 Chrome Debug 技巧

<!-- more -->

首先建立一個 html 檔案，並在 `<script>` 中加入一個 `debugger` 關鍵字，接著下方可以隨意方進一段範例程式碼

```html
// test.html
<script>
  debugger
  let arr = [2, 7, 5];
  let max = arr[0];
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] > max) max = arr[i];
  }
  console.log(max);
</script>
```

如果是透過 VSCode 可以透過 `Live Server` 這個 Extension 開啟，或者也可以直接點擊 html 檔案於瀏覽器開啟，開啟後按 `F12` 開啟開發者工具會自動跳轉到 `Sources`，並顯示 `html` 檔案中的程式碼，且會停留在 `debugger` 這個關鍵字上

![](Sources.PNG)

接著看到右邊會有一排按鈕，每個按鈕代表一種接下來程式碼執行的方式，最常用的 `Step F9`，按一下執行一步程式碼，依序一行一行執行

![](Step.png)

點選 `Step` 會發現右側 `Scope` 中會根據當下執行的狀態顯示變數的值，我們就可以藉此為程式碼 Debug 囉

![](Scope.png)


