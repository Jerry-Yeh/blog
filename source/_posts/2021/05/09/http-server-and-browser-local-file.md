---
title: http-server and browser local file
abbrlink: 2556
date: 2021-05-09 15:56:57
tags:
- http-server
- browser
categories: Browser
thumbnail:
banner:
description:
---

`http-server` 基礎應用與 `browser` 開啟本地端檔案

<!-- more -->

### 問題

在工作上遇到一個暫時性需求需要將前端 build 出來的檔案在本地端開啟，但無論是 webpack 或是基礎專案，只要不是全寫在 `html` 中，就會透過 link 或 script 來載入其他 `js` 或 `css` 檔案，或是嘗試使用 `ajax` 來取得本地端檔案，會發現網頁打開一片空白，開啟 console 會出現以下 error log

```
Cross origin requests are only supported for protocol schemes: http, data, chrome, chrome-extension, https, chrome-extension-resource.
```

原因為該行為在 chrome 是被禁止的，是為了避免檔案內容被更改而載入惡意程式碼，網路上有些文章提及使用 firefox 可以解決，但我在這篇文章中的時間點使用 firefox 會出現相同的情況

### 解法

#### 解法一

透過 CMD 重新開啟 chrome ，但這個方法為一次性的，每次開啟 chrome 都要重新補上這段 CMD，以此來讓 chrome 忽略載入 local file 這個動作

```bash
chrome.exe --allow-file-access-from-file 
```

#### 解法二

架一個最簡單的 `http-server`，透過 `npm` 即可安裝 `http-server` 這個套件，只要在對應資料夾路徑底下執行，就可以在該路徑開啟任何 `html` 檔案

```bash
# install on global
npm i http-server -g

# open http server
http-server
```

接著就可以在 browser 開啟，預設為 `8080` port

```
http://localhost:8080/index.html
```

