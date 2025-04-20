---
title: Scss import
abbrlink: b8cb8afb
date: 2019-09-24 23:07:50
tags: Scss
categories: Scss
thumbnail:
banner:
description:
---

透過 Sass 中的 `@import` 功能，可以將檔案拆分成許多支，最後再透過此功能彙整，以方便管理

<!-- more -->

## @import 的用法

* 使用 `@import` 可以將 "多支 Sass" 檔案彙整成 "一支 Sass" 檔案，再透過轉譯轉換成 CSS 檔案，因此載入時只要載一支 CSS 檔案即可
* 在 Sass 檔名前面加上下底線 `_`，此檔案就不會被編譯成 CSS 檔案，例如: `_reset.scss`
* 將 CSS 拆分成多支 Sass 檔案，使 CSS 程式碼切分模組化，每支檔案的程式碼較少，使維護上更能精準的聚焦
* [Bootstrap](https://github.com/twbs/bootstrap/blob/master/scss/bootstrap.scss) 就是用這種方式在管理眾多的 CSS 程式碼
* 如果我們要將變數檔案 `_variable.scss` 與首頁檔案 `index.scss` 載入到總檔案 `all.scss` 中，就可以透過以下方法
  
  ``` scss
  // in all.scss
  @import "variable";
  @import "index";
  ```

* 注意載入時不需要加最前面的下底線 `_` 和最後面的副檔名 `.scss`，並在每行結尾用分號 `;` 隔開
* 因為 Scss 檔案是 "由上而下" 讀取，所以會被其他檔案使用到的檔案要優先匯入，例如 `_variable.scss` 和 `_mixin.scss`
* 通常都是將所有檔案匯入 `!import` 到一支名為 `all.scss` 的檔案管理，再將這支檔案編譯成瀏覽器可以讀取的 `all.css`

## CSS reset

* 通常在寫網頁時，我們都會載入一支名為 _reset.scss 的檔案來淨空所有樣式
* 要完全靠自己完成 `_reset.scss`，會比較麻煩，所以通常都會載入網路上已寫好得程式碼，常見的有兩種，`meyerweb` 和 `Normalize`

  * [meyerweb](https://meyerweb.com/eric/tools/css/reset/) : 完全淨空所有樣式
  * [Normalize](https://necolas.github.io/normalize.css/) : 保留部分樣式，不會完全淨空所有樣式

* 選擇性 : 如果是工程取向，建議使用 `meyerweb` 完全淨空，設計取向可以使用 `Normalize` 保留部分樣式，`Bootstrap` 即是使用 `Normalize`，在載入後仍然可以依照需求作修改
