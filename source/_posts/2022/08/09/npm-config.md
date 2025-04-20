---
title: NPM config
abbrlink: 40501
date: 2022-08-09 23:29:51
tags:
categories:
  - npm
thumbnail:
banner:
description:
---

通常在工作上，為了顧及資安問題，通常會透過使用內網以保護公司內部的開發資源與訊息，但也因為這個原因導致許多網站與開發資源被拒於防火牆外，這次在工作上遇到一個非常直接也很困擾的問題是無法直接在內網安裝 npm 相關套件，還好 npm 提供了設定方式以一次性的解決 http/https 與 proxy 的連線問題

<!-- more -->

在公司內部，通常會連線至內網處理所有工作，再由 proxy server 連線至 internet，這次遇到一個很困擾的問題是所有 JavaScript 在開發時必定會使用到的 npm 套件庫，在對 npm 進行設定前，只能偷偷連線到外網以下載對應的資源，但其實 npm 提供了幾個連線設定以解決這個問題

- 查看 npm 設定

  ```bash
  $ npm config list
  ```

- 設定 npm proxy，這裡比較特別的是，http 的指令為 proxy，https 則為 https-proxy，後方同樣帶入 proxy server url

  ```bash
  $ npm config set proxy http://company.proxy.com:8080
  $ npm config set https-proxy http://company.proxy.com:8080
  ```

- 如果公司內還需要帳號密碼以連線至 proxy，只要將帳號密碼加在 proxy url 前方即可

  ```bash
  $ npm config set proxy http://${name}:${password}@company.proxy.com:8080
  ```

- 而如果不希望密碼被直接透過 npm config get 顯示出來，可以將密碼透過 base64 轉譯，仍能成功連線

  ```bash
  $ npm config get <key>
  ```

其他相關指令可以參考 [NPM 官網](https://doc.codingdict.com/npm-ref/cli/config.html)

## 資料參考

[npm-config](https://doc.codingdict.com/npm-ref/cli/config.html)
