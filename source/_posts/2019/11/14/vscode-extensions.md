---
title: VSCode Extensions
abbrlink: 7433
date: 2019-11-14 01:01:46
tags: VSCode
categories: VSCode
thumbnail:
banner:
description:
---

這篇文章記錄一下目前在 VSCode 中使用到的擴充插件

<!-- more -->

## Chinese (Traditional) Language Pack for Visual Studio Code

VSCode 中提供了許多符合本地化語言的擴充插件，這個插件可以將整個 VSCode 環境改成繁體中文，只需要按照以下官方說明的使用方式即可更改，更改後只要重啟 VSCode 就能完成

### 使用方式

安裝好之後，在 `locale.json` 裡面設定 "locale": "zh-tw" 即可載入中文 (繁體) 的語言套件。要修改 `locale.json`，可以按下 `Ctrl+Shift+P` 來呼叫 命令選擇區，輸入 "config" 以從其他可用命令裡面篩選出 `Configure Language` 命令。詳細步驟請參考文件 。

## favorites

這個插件提供開發者一個獨立的區域將指定檔案加到其中，就可以快速的找到指定檔案，只要對目標檔案案右鍵選取 `add to favorites (添加到收藏)`

## indent-rainbow

這個插件會將程式碼前的空白透過顏色來區分，如此一來就可以快速的對齊飛相鄰的程式碼

## Live Sass Compiler

當我們使用 CSS 預處理器 `Sass` 時，這個插件可以即時將 `.scss` 與 `.sass` 編譯成瀏覽器可以讀取得 `.css` 檔案格式

## Live Server

安裝完此插件後，會在 VSCode 下方出現 Go Live 的選項，點選後我們在編輯網頁時就會及時在瀏覽器上改變，使我們快速的看到效果更動，不需要再次重新整理整個網頁

## Path Intellisense

這個插件提供我們再輸入 "相對路徑" 時，出現對應的路徑選項，不需要將整個路徑從頭到尾完整輸入







