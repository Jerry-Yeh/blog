---
title: Scss 條件與迴圈
tags: Scss
categories: Scss
abbrlink: ff62e7a9
date: 2019-09-25 00:03:37
thumbnail:
banner:
description:
---

在 Sass 中，也新增了與一般程式語言類似的條件判斷與迴圈，使 CSS 的運用更加靈活

<!-- more -->

## Conditions 條件運算

* Sass 的條件運算與一般程式語言的概念相同，時常會搭配 `@function` 與 `@mixin` 組合使用

  ``` scss
  @if(條件){
  ...
  }@else if(條件){
    ...
  }@else (條件){
    ...
  }
  ```

* 以下為結合 `@mixin` 做出三角形的作法，使用條件運算產出不同方位的三角形，`$type:1` 為等腰三角形，`$type:0.86666` 為正三角形

  ``` scss
  /* in scss */
  @mixin triangle ($size, $color, $align,$type:1){
    height: 0;
    width: 0;
    @if  ($align == top) {
      border-bottom: ($size*$type) solid $color;
      border-left: ($size/2) solid transparent;
      border-right: ($size/2) solid transparent;
    }@else if  ($align == right) {
      border-left: ($size*$type) solid $color;
      border-top: ($size/2) solid transparent;
      border-bottom: ($size/2) solid transparent;
    }@else if  ($align == bottom){
      border-top: ($size*$type) solid $color;
      border-left: ($size/2) solid transparent;
      border-right: ($size/2) solid transparent;
    }@else if  ($align == left){
      border-right: ($size*$type) solid $color;
      border-top: ($size/2) solid transparent;
      border-bottom: ($size/2) solid transparent;
    }
  }

  .box {@include triangle(50px, red, top)}

  /* in css */
  .box {
    height: 0;
    width: 0;
    border-left: 50px solid red;
    border-top: 25px solid transparent;
    border-bottom: 25px solid transparent;
  }
  ```

* 現在 CSS 也支援條件運算

  ``` css
  .parent {width: if(條件, 條件成立帶入的值, 條件不成立帶入的值);}
  ```

## Loops 迴圈運算

### @each 迴圈

`@each` 可以針對陣列 `list` 和物件 `map` 中的每一項進行運算，藉由變數 `$item` 動態帶入每一項進入迴圈

* @each 語法

  ``` scss
  @each $var in "map or list" {...}
  ```

* list 範例

  ``` scss
  /* in scss */
  $list: (red, blue, green);

  @each $item in $list {
    .#($item){
      background: $item;
    }
  }

  /* in css */
  .red {background: red};
  .blue {background: blue};
  .green {background:green}
  ```

* map 範例，此方式也是 Bootstrap 在管理變數的方法

  ``` scss
  /* in scss */
  $theme-colors: (
    'primary': blue,
    'secondary': #6c757d;
  );

  @each $name, $value in $theme-colors {
    .bg-#{$name} {
      background: $value;
    }
  }

  /* in css */
  .bg-primary {background: blue;}
  .bg-secondary {background: #6c757d;}
  ```

### @for 迴圈

Sass 的 `@for` 迴圈運算與一般程式語言的 `for` 概念相同，須給定 "起始值" 和 "終止值"

* `@for` 語法

  * `$i` 變數代表 `map` 和 `list` 項目的編號

  * `$begin` 和 `$end` 代表起始值與終止值

  * 關鍵字 `through` 代表從包含起始值到終止值，使用 `to` 的話則從 "起始值" 開始到 "終止值前一位"

  ``` scss
  @for $i from $begin through $end {...}
  ```

* @for 範例: 產生顏色漸變

  ``` scss
  /* in scss */
  @for $i from 1 through 3{
    .box-#{$i}{
      background-color: darken(white, $i * 10%);
    }
  }

  /* in css */
  .box-1 {background-color: #e6e6e6;}
  .box-2 {background-color: #cccccc;}
  .box-3 {background-color: #b3b3b3;}
  ```