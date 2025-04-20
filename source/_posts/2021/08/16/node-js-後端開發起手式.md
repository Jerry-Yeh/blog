---
title: Node.js 後端開發起手式 (一)
abbrlink: 22460
date: 2021-08-16 23:55:09
tags: 
  - Node
  - Back-end
categories:
thumbnail:
banner:
description:
---

[偷米騎巴哥] Node.js 後端開發起手式紀錄，Node.js 基本介紹

<!-- more -->

### 目前已知的 JS 運用

- 瀏覽器 ( Chrome、Firefox、Safari、IE、etc. )
- 前端工具 ( Webpack、Gulp、Grunt、etc. ) 
- 後端開發 ( Express、Koa、etc. )
- 資料庫 ( MongoDB )
- 硬體版 ( Raspberry Pi、Webduino、etc. )
- Google 雲端服務 ( Google Apps Script )

### 安裝 Node.js

安裝具備以下兩種方式

#### 透過 NVM 安裝

- 優點
  - 快速切換不同版本
  - 方便測試
- 缺點
  - 占用較多磁碟空間 ( 但以當今的硬體規格其實可以忽略這一點安裝空間 )

可以透過到官方的 [Github](https://github.com/coreybutler/nvm-windows/releases) 下載，一般下載 nvm-setup.zip 版本接下來一路按確認即可安裝。

安裝完 NVM 後接著就是打開 CML 透過 NVM 來安裝 Node.js，輸入 nvm 就會出現許多功能介紹。

安裝 Node.js 特定版本

```bash
nvm install <version>
```

使用、切換下載後的 Node.js 版本

```bash
nvm use
```

#### 直接安裝

- 優點
  - 於官網直接安裝
  - 版本固定
- 缺點
  - 切換版本需重新安裝

Node.js 官網後會看到有兩個版本可以下載，方別是 LTS ( Long Time Support ) 長期支援的版本 & Current 最新版本，如果沒有對於最新版本的需求，官方建議下載 LTS 版本即可，此版本大版號下官方宣稱會持續維護三年左右。

基於以上幾點比較，現在的環境較為推薦使用 NVM 進行安裝。

### 基本介紹

Node.js 是 JS 在 Node 上執行的方式，也因為載體不再是瀏覽器，因此也缺乏 DOM & BOM 的操控。但與前端瀏覽器相同的是，都採用 Chrome 團隊 V8 引擎，號稱地表最快 JS 引擎，原因在於一般程式語言需要被電腦讀取都需要進行編譯的動作轉成中介碼，最後再轉成電腦可直接讀取的機械碼，但 V8 省去中介碼的轉換因此能有較好的效能。
一般來說大版號基數為開發測試版，偶數則為正式版號，因此都以偶數版為下載版本。

#### REPL (Read Eval Print Loop) 交互式命令

REPL 其實就和前端瀏覽器中的 console 一樣，可藉由輸入 console 印出對應訊息，作法是在 CML 中輸入 `node` 後，在顯示的 `>` 符號後輸入以往在前端輸入的 console 訊息即可，但也因為不在瀏覽器中，所以同樣無法使用 DOM & BOM。

```bash
node
```

##### 執行 JS 檔案

亦可直接執行 JS 檔案，副檔名 `.js` 可以直接忽略

```bash
# 執行 demo.js
node demo
```

##### 跳出 REPL

按一下 `ctrl + D` 或兩下 `ctrl + C` 都可以跳出 REPL 狀態

### 全域

在瀏覽器中如果直接印出 `this` 會出現 `Window` 物件，因為此物件為瀏覽中的最高級物件。一段最簡單的程式碼如下，宣告一個變數後，透過 `this` 呼叫會發現，透過 browser console & REPL 都會出現期待的結過，但直接透過 node 呼叫會出現 `undefined`

``` js
var a = 100;

console.log(this);

function run() {
  console.log(this.a);
}

run();

// browser console & REPL

{}
100

// node

global
undefined

```

#### global

其原因是在 node 中，為了避免全域之間彼此互相汙染，所以檔案內宣告的全域變數並不會直接掛載到全域 `global` 上，但 REPL 因為每次開啟相互獨立，所以不會有這個問題

```js
// demo.js
console.log(this === global);

// node demo
false

// REPL
console.log(this === global)   // true

```

##### global 屬性

以下幾個屬性是專屬於 node.js，無法在瀏覽器中使用

- require()
  - 載入模組 ( [Node.js 內建模組](https://nodejs.org/dist/latest-v14.x/docs/api/fs.html) )
    ```js
    // abc.txt
    abcdef

    // demo.js
    const fs = require('fs');
    var data = fs.readFilesSync('abc.txt'); 
    console.log(data);
    ```

    載入後會發現出現一段 `buffer`，需要透過 `toString` Function 來將其轉成一般字串

    ```js
    // demo.js
    const fs = require('fs');
    var data = fs.readFilesSync('abc.txt'); 
    console.log(data.toString());
    ```
  - 載入套件 ( 透過 NPM 下載 )
    和前端一樣在專案底下執行指令即可安裝
    ```bash
    npm i <plugin name>
    ```
    載入後會發現所有 npm 相關套件都會被安裝在 `node_modules` 資料夾下

    **package.json**

    使用 npm 套件時，可透過以下指令來建構 package.json 作為專案套件的目錄
    ```bash
    npm init
    ```
    輸入後會詢問一些專案基本資料，如果希望跳過這些問題使用預設值，在後方加上 `-y` 即可
    ```bash
    npm init -y
    ```

    **package.lock.json**

    被記錄在這個檔案中的套件會使下次在下載專案時載入指定版本，避免有些套件橫跨大版號時語法不完全相容導致程式無法執行，因此這個檔案一般也會 commit 到 git 中。npm 5.0 版本開始就會自動產生這個檔案

  - 載入自訂模組 ( 自己寫的模組 )

- exports
  在製作一個模組時，可以透過 `exports` 供其他模組使用，模組預設為一個 '物件'，所以一般來說會在其之下新增屬性。需要特別注意的是，這裡的 require 需使用 '相對路徑'。

  ```js
  // module1.js
  exports.run = function() {
    console.log('run');
  }

  // demo.js
  var m1 = require('./module1');
  m1.run();   // run
  ```

  **module.exports**

  然而如果不想從一個物件為起點，不能直接將 `exports` 賦予另一個物件，需使用 `module.exports` 來指向 `exports`，但 `require` 其實是指向 `module.exports`
  ```js
  // module1.js
  module.exports = function() {
    console.log('run');
  }

  // demo.js
  var m1 = require('module1');
  m1();   // run
  ```

- module
  如同前面所述，每個 `exports` 的 .js 檔案都可以視為一個 module
- __filename
  目前正在執行的檔案
- __dirname
  目前正在執行的資料夾
- process
  提供全域資訊與訊息控制，以下為長裕資訊屬性
  - process.argv
    這個屬性會顯示執行時所代的參數，預設會有 node.js 和該檔案本身
    ```bash
    [
      'C:\\Program Files\\nodejs\\node.exe',
      'C:\\Users\\YA\\Desktop\\Node\\demo'
    ]
    ```
    如果在後方帶入額外參數則會往後新增，多個參數間只要透過空格方開即可
    ```bash
    node demo xxx yyy

    [
      'C:\\Program Files\\nodejs\\node.exe',
      'C:\\Users\\YA\\Desktop\\Node\\demo',
      'xxx',
      'yyy'
    ]
    ```
  - process.env
    為一個代表環境變數的物件，包含路徑、OS.etc
  - process.cwd
    代表執行當下的工作目錄





