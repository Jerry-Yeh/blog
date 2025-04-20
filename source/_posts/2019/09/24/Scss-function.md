---
title: Scss function
tags: Scss
categories: Scss
abbrlink: b33be25b
date: 2019-09-24 23:37:28
thumbnail:
banner:
description:
---

如同一般程式語言中的函式 `function` ，Sass 中也提供了類似的方式，而 `@function` 與 `@mixin` 不同之處在於其本身就有已經設定好的函式供我們使用，稱為 "內置函式"

<!-- more -->

## @function 回傳值

藉由呼叫來使用函式，其本身代表的是一個 "值" ，而不是一個 CSS 樣式，所以無法單獨使用

``` scss
// 直接使用為錯誤用法
darken(#ff3366, 50%);

// 作為"變量值"帶入變數中
$redDark: darken(#ff3366, 50%);

// 作為"屬性值"帶入 CSS 屬性中
p {color: darken(#ff3366, 50%);}
```

## 內置函式

Sass 本身就有提供許多已經寫好的函式供我們使用，以下為內置函式的分類

* ### 字串函式
  [Sass 學習雜記 - A3. 字串函式](https://yungshenglu.github.io/2018/04/30/SassNotesA3/)

* ### 數學函式
  [Sass 學習雜記 - A1. 數學函式](https://yungshenglu.github.io/2018/04/30/SassNotesA3/)

* ### 顏色函式

  常見實用的顏色函式有提高亮度 `lighten`、降低亮度 `darken` 、提高飽和度 `saturate` 、降低飽和度 `desaturate` 和改變色相值 `adjust-hue`，可以透過以下連結調色，且不同項目的顏色函式可以疊加使用

  [Sass 顏色工具｜SassMe](https://sassme.jim-nielsen.com/)
  [Sass 學習雜記 - A2. 顏色函式](https://yungshenglu.github.io/2018/04/30/SassNotesA2/)

## 自訂函式

除了以上實用的內置函式，我們也可以自訂想要的函式，函式內部使用 `@return` 回傳結果到函式名稱，例如將 `px` 轉成 `rem` 的函式

``` scss
/* in scss */
@function pxToRem($px, $base: 16){
  @return ($px / $base) * 1rem;
};

p {font-size: pxToRem(20);}

/* in css */
p {font-size: 1.25rem;}
```

## 資料參考

  [Sass 學習雜記 - A3. 字串函式](https://yungshenglu.github.io/2018/04/30/SassNotesA3/)
  [Sass 學習雜記 - A1. 數學函式](https://yungshenglu.github.io/2018/04/30/SassNotesA3/)
  [Sass 學習雜記 - A2. 顏色函式](https://yungshenglu.github.io/2018/04/30/SassNotesA2/)





