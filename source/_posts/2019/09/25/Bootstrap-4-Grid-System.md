---
title: Bootstrap 4 Grid System
tags: Bootstrap
categories: Boostrap
abbrlink: 99a4840c
date: 2019-09-25 01:42:43
thumbnail:
banner:
description:
---

在 Bootstrap 中，設計了相當完善的響應式格線系統，即使沒有要使用其中的模組化框架，也可以單獨使用格線系統，而且只要修改設定檔，就可以輕鬆做出客製化的響應式格線系統 

<!-- more -->

## 響應式斷點

Bootstrap 是設計成行動 (手機) 優先的框架，並透過 `media queries` 來設立中斷點

### 預設斷點

Bootstrap 的預設斷點

``` scss
/* Small devices (landscape phones, 576px and up) */
@media (min-width: 576px) { ... }

/* Medium devices (tablets, 768px and up) */
@media (min-width: 768px) { ... }

/* Large devices (desktops, 992px and up) */
@media (min-width: 992px) { ... }

/* Extra large devices (large desktops, 1200px and up) */
@media (min-width: 1200px) { ... }
```

### 斷點變數

Bootstrap 預設的斷點變數如下，我們可以透過修改`_variable.scss` 中的變數來新增或刪除斷點

``` scss
$grid-breakpoints: (
  xs: 0,
  sm: 576px,
  md: 768px,
  lg: 992px,
  xl: 1200px
) !default;
```

### @mixin

我們也可以透過 Bootstrap 寫好的 `@mixin` 使用最大、最小或特定螢幕尺寸的其中一段

``` scss
/* @media (max-width: 575.98px) */
@include media-breakpoint-down(xs) { ... }

/* @media (min-width: 576px) */
@include media-breakpoint-up(sm) { ... }

/* @media (min-width: 576px) and (max-width: 1199.98px) */
@include media-breakpoint-between(sm,xl) { ... }

/* @media (min-width: 576px) */
@include media-breakpoint-between(sm,lg) { ... }

/* @media (min-width: 576px) and (max-width: 767.98px) */
@include media-breakpoint-only(sm) { ... }
```

## Grid System

Bootstrap 中的格線系統

### 基本架構

``` html  
<div class="container">
  <div class="row">
    <div class="col">
      <!-- 內容 -->
    </div>
  </div>
</div>
```

* 透過最外層的 `.container`，為每個斷點提供固定的容器寬度 ; 如果想要滿版寬度，在這層可以使用 `.container-fluid`
* 第二層的 `.row` 為橫列，用來包住內層的欄 `.col`，確保每個欄位排列適當
* 所有內容都放在第三層欄 `.col` 的 內層，而不是直接撰寫樣式在 `.col`上，`.col` 也只能是 `.row` 的直接子類

### 格線變數

* 預設的欄位數 `12` 與欄間距 `30px`，我們可以在 `._variable.scss` 找到以下相關變數來修改成想要的格式

  ```scss
  $grid-columns: 12 !default;
  $grid-gutter-width: 30px !default;
  ```

* `.container` 在每個斷點都有預設寬度，同樣的我們也可以在
 `._variable.scss` 中找到相關變數來修改成想要的寬度

  ``` scss
  $container-max-width: (
    sm: 540px,
    md: 720px,
    lg: 960px,
    xl: 1140px
  ) !default;
  ```

### container 容器

以下為 `.container` 的預設樣式

``` css
.container {
  margin-right: auto;
  margin-left: auto;
  padding-right: 15px; /* $grid-gutter-width */
  padding-left: 15px;  /* $grid-gutter-width */
  width: 100%;
  max-width: 1140px;　/* 隨螢幕尺寸而變 */
}
```

* `margin-right: auto` 和 `margin-left: auto` 為水平置中
* 因為有 `padding`，所以 `.container` 的實際寬度為 `max-width` 減 `$grid-gutter-width`，這個斷點下為 `1110px`
* 如果是 `.container-fluid` 就沒有設定 `max-width`，所以會自適應延伸滿版寬

### row 列

以下為` .row` 的預設樣式

``` css
.row {
  display: flex;
  flex-wrap: wrap;
  margin-right: -15px; /* $grid-gutter-width */
  margin-left: -15px; /* $grid-gutter-width */
}
```

* `.row` 使用 `flex` 來排版，而 `flex` 的預設為水平排列
* 負值的 `margin` 將 `.container` 的 `padding` 外推，所以 `.container` 的內容寬度在此段點變回 1140px
* 因為 `.row` 是使用 `flex` 來排版，所以可以直接使用 `flexbox` 來操作內容，Bootstrap 已經有寫好的 class

  ``` html  
  <div class="row justify-content-center align-items-end">
    <div class="col"></div>
    <div class="col"></div>
  </div>
  ```

### col 欄

以下為 `.col` 的預設樣式

``` css
.col {
  position: relative;
  flex-basis: 0;
  flex-grow: 1;
  max-width: 100%;
  width: 100%;
  min-height: 1px;
  padding-right: 15px;
  padding-left: 15xp;
}
```

* 因為有 `padding`，所以此斷點的內容寬度又變回 `1110px`
* 因為是使用 `flex` 來排版，所以預設每個欄位是等寬的

  ``` html  
  <div class="row">
    <div class="col"></div>
    <div class="col"></div>
    <div class="col"></div>
  </div>
  ```

* 當然也可以指定 `.col` 的寬度，總和超過 `12` 就會自動換行

  ``` html  
  <div class="row">
    <div class="col-4"></div>
    <div class="col-7"></div>
    <div class="col-3"></div>
  </div>
  ```

#### 在不同螢幕尺寸下設定不同的寬度

``` css
.col-"螢幕尺寸"-"欄位數"
```

* Extra small ( `< 576px` ) : `.col-`
* Small ( `≥ 576px` ) : `.col-sm-`
* Extra small ( `≥ 768px` ) : `.col-md-`
* Large ( `≥ 992px` ) : `.col-lg-`
* Extra large ( `≥ 1200px` ) : `.col-xl-`

Bootstrap 的響應式是 "行動版優先"，也就是 "由小到大"，以下為範例說明

  ``` html  
  <div class="row">
    <div class="col-12 col-sm-6 col-md-3"></div>
  </div>
  ```

* 欄位預設為 `12`
* 螢幕尺寸 `≥ 576px` 時，欄位為 `6`
* 螢幕尺寸 `≥ 768px` 時，欄位為 `3`

### offset 欄的推移

如果欄的位移空間與欄間隔相符合，我們就可以使用 `offset` 向左推移出空欄位

``` css
.offset-"螢幕尺寸"-"推移欄位數"
```

以下範例為在螢幕尺寸 `≥ 768px` 時，向左推移 `4` 欄位

``` html  
<div class="row">
  <div class="col-3 offset-md-4"></div>
</div>
```

## Utilities 通用類別

在 Bootstrap 中，所有共用的變數都會放在 Utilites 這個分類中，以下介紹幾個常見的類別

### spacing 間隔

* #### 變數

  我們可以透過 `_variable.scss` 來修改間距數值，除了新增和刪除，也可以修改 "單位"

  ``` scss
  $spacer: 1rem !default;
  $spacers: () !default;

  $spacers: map-merge (
    (
      0: 0;
      1: ($spacer * .25),
      2: ($spacer * .5),
      3: $spacer,
      4: ($spacer * 1.5),
      5: ($spacer * 3)
    ),$spacers
  );
  ```

* #### 符號

  ``` cs
  "屬性""方位"-"螢幕尺寸"-"數值"
  ```

  * 屬性 : 
  `.m` 代表 `margin`
  `.p `代表 `padding`

  * 方位 : 
  `t` 代表 `top`
  `r` 代表 `right`
  `b` 代表 `bottm`
  `l` 代表 `left`
  `x` 代表 `left` 和 `right`
  `y` 代表 `top` 和 `bottom`
  沒有給值則是四邊全方位

  * [螢幕尺寸:](#在不同螢幕尺寸下設定不同的寬度) 
  上方已提過，點選回去查看

  * 數值 :
  `0` 代表 `歸零`
  `auto` 代表 `auto`
  其餘數值由上方變數定義，預設為 `1 ~ 5`

### flex

Bootstrap 4 使用 `flex` 來排版，並將所有樣式寫成 class 名稱，載入 `_utilities.scss` 並了解其命名規則就可以靈活運用，詳細參考以下連結

[通用類別 - Flex](http://bootstrap.hexschool.com/docs/4.2/utilities/flex/)

## 資料參考

[Bootstrap](https://getbootstrap.com/)
[Bootstrap 中文版](https://bootstrap.hexschool.com/)
[Bootstrap on Github](https://github.com/twbs/bootstrap)
