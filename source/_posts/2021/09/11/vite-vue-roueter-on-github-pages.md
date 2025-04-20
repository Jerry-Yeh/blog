---
title: Vite + Vue-Roueter on Github Pages
abbrlink: 13819
date: 2021-09-11 10:50:27
tags:
  - Bug
  - Vue
  - Vite
  - Vue Router
categories:
  - Vue 3
thumbnail:
banner:
description:
---

紀錄使用 Vite + Vue 3 + Vue-Router 部屬到 Github Pages 所遇到的問題

<!-- more -->

### 部屬到 Github

不論是使用 Vite 或 Vue CLI 建構專案，在部屬到 Github Page 時，如果是要部屬到額外的 Repository，都需要在生成的讀取檔案路徑前加上 Repository 名稱，但只限於部屬到 Github 上 build 出後的檔案。

#### 設定
可透過以下兩種方式區分開發模式 (dev) 與生成 (build) 的檔案

- 環境設定檔
在專案跟目錄新增一個環境設定檔，如果是 Vite 就是 `vite.config.js`，Vue CLI 則是 `vue.config.js`， 在 `vite.config.js` 有一個參數 `base` 可以設定檔案的路徑，並透過 `process.env.NODE_ENV` 來判斷 Server 執行在開發或正式環境中，如果是正式環境則須加上 Github Respository 名稱

  ```js
  // vite.config.js

  import { defineConfig } from 'vite';
  import vue from '@vitejs/plugin-vue';
  import path from 'path';

  // https://vitejs.dev/config/
  export default defineConfig({
    plugins: [vue()],
    build: {
      minify: true,
    },
    resolve: {
      alias: {
        '@': path.resolve(__dirname, './src'),
      },
    },
    base: process.env.NODE_ENV === 'production' ? '/<REPO>/' : './',
  });
  ```

- 於生成正式檔案時加上
另外也可以透過在生成正式檔案時加上，如果是透過 vite 建置專案，可以在 `package.json/scripts/build` 後加上 `--base=/<REPO>/` 參數藉此在生成時附加 Repository 路徑

  ```js
  // package.json

  {
    "name": "...",
    "version": "0.0.0",
    "scripts": {
      "dev": "vite",
      "build": "vite build --base=/<REPO>/",
      "serve": "vite preview"
    },
    "dependencies": {
      ...
    },
    "devDependencies": {
      ...
    }
  }
  ```

#### 部屬

在 [官方的文件](https://vitejs.dev/guide/static-deploy.html#github-pages) 中推薦使用建立一個 `deploy.sh` 的 Shell Script 檔案來一鍵生成、部屬到 Github `gh-pages` branch 上，在 Github 中，Github Pages 會優先讀取 `gh-pages` branch 中的檔案，因此可藉此區分開發與生成的檔案

```sh
#! deploy.sh

# abort on errors
set -e

# build
npm run build

# navigate into the build output directory
cd dist

# if you are deploying to a custom domain
# echo 'www.example.com' > CNAME

git init
git add -A
git commit -m 'deploy'

# if you are deploying to https://<USERNAME>.github.io
# git push -f git@github.com:<USERNAME>/<USERNAME>.github.io.git master

# if you are deploying to https://<USERNAME>.github.io/<REPO>
# git push -f git@github.com:<USERNAME>/<REPO>.git master:gh-pages

cd -
```

### Vue Router
在 Vue 3 所搭配使用的 [Vue CLI 4](https://next.router.vuejs.org/guide/essentials/history-mode.html#different-history-modes) 中，提供了一個 `history` 參數來選擇前端路由的顯示模式，方別是 `Hash Mode` 和 `HTML5 Mode`

- Hash Mode
這是在 Vue CLI 4 以前前端路由所使用的方式，會加網址後加上一個 `#` ，但也因為這樣會降低瀏覽器的 SEO ，因此官方建議如果注重 SEO 的網站可以使用第二種 `HTML5 Mode`

  ```js
  // router.js

  import { createRouter, createWebHashHistory } from 'vue-router'

  const router = createRouter({
    history: createWebHashHistory(),
    routes: [
      //...
    ],
  })
  ```

- HTML5 Mode
將 history 切換到這個模式後就會在網址省略 `#`

  ```js
  // router.js

  import { createRouter, createWebHistory } from 'vue-router'

  const router = createRouter({
    history: createWebHistory(),
    routes: [
      //...
    ],
  })
  ```

#### 問題

但在 Github page 使用 `HTML Mode` 就會發現當 Router 切換到其他路徑時，就會跳轉到 404 Error 頁面，原因是 Vue 只是切換 Component 而已，路徑並不存在，這裡有以下三個方式可以解決這個問題

- 404.html
在 `index.html` 跟目錄位置新增一個 `404.html`，內容只要完全複製 `index.html` 即可

- Hash Mode
將 Vue router history 切換回 `Hash Mode` 也可以解決這個問題

- 實作一個簡單的重新導向機制，在出現 404 頁面時指向 `index.html`，此原理和第一種解法相同


