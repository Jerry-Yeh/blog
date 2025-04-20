---
title: Scss extend
abbrlink: 1a508081
date: 2019-09-24 23:29:14
tags: Scss
categories: Scss
thumbnail:
banner:
description:
---

為了不要使 CSS 太過龐大，遇到相同樣式時，都會將其合併起來，但這會遇到一個問題，如果開發到後面程式碼太多，遇到相同樣式要合併時又得拉到前面找出來合併

<!-- more -->

``` css 
/* 第20行 */
.header h1, .content h1, .footer h1 {...} 
/* 第400行 */
.content h1 {...}
/* 第4000行 */
.footer h1 {...}
```

## @extend 基本運用

透過 `@extend` 可以將其他的 class 拉進來合併延伸使用

``` scss
/* in scss */
.box1 {color: red}
.box2 {
  width: 100px;
  @extend .box1;
}

/* in css */
.box1, .box2 {color: red}
.box2 {width: 100px}
```

須注意合併樣式的先後順序，以下情況 `.box2` 的 `border-color` 樣式就會被覆蓋

``` scss
/* in scss*/
.box2 {
  @extend .box1 ;
  border-color: blue;
}
.box1 {border: 1px solid red}

/* in css */
.box2 {border-color: blue;}
.box1, .box2 {border: 1px solid red}
```

## % 專門被呼叫的樣式

可以用 `%` 建立一個樣式，只有使用 `@extend` 呼叫時才會作用，本身並不會被 Sass 編譯成 CSS

``` scss
/* in scss */
%all-h1 {color: red}
.header {
  width: 100px;
  @extend %all-h1
};
.footer {
  width: 200px;
  @extend %all-h1
};

/* in css */
.header, .footer {color: red};
.header {width: 100px};
.footer {width: 200px}
```

## @extend 與 @mixin 的差異

* ### @extend

  主要用於樣式的合併，無法帶入參數，如果樣式固定，使用 `@extend` 程式碼會較為簡潔

* ### @mixin

  將整包程式碼帶到對應的 `class` 內，可以帶入參數，適合用於多變數運算
