---
title: Scss 變數
tags: Scss
categories: Scss
abbrlink: 4740e2f1
date: 2019-09-24 22:39:39
thumbnail:
banner:
description:
---

## 為什麼需要變數 ?

變數可以設定全域樣式，如果我們要改變專案中所有連結的顏色，純 CSS 只能一一去做更改，但將所有連結顏色的值導入變數後，只需要改變變數的值所有連結顏色就會一起更改

<!-- more -->

``` scss
$link-color: red;
.header a {color: $link-color;}
.menu a {color: $link-color;}
```

## 變數的格式

變數可以是任何 CSS 屬性的值，只要在最前面加上 `$`，接著再加上命名的名稱，最後以 `;` 結束

### 數值

可以包括單位和小數，其中單位可以是絕對單位 (px)

``` scss
$z-index-l: 10;
$font-size-base: 16px;
```

### 字串

可以用 雙引號 `""`、單引號 `''` 或沒有引號，是一個字串列時，可以用 `,` 或空白隔開

``` scss
$font-family: "Noto Sans TC",sans-serif;
$brand-img: '../img/brand.png';
```

### 顏色

可以是各種代碼表現

```scss
$white: white;
$primary: #fff;
$secondary: rgb(0,0,0, .5)
```

### 布林值

`true`, `false`

### 空值

`null`

## 變數的運算

Sass 支援基本的數學運算，加 `+`、減 `-`、乘 `*`、除 `/`

``` scss
$font-size-base: 16px;
$font-l: $font-size-base * 1.2;
$font-s: $font-size-base * 0.8;
```

如果遇到除不盡的的小數會運算網頁解析度可以容許的範圍，雖然有很微小得留白，但肉眼幾乎無法察覺

``` scss
//in .scss
$width: 960px;
.menu {width: $width / 7}
// in .css
.menu {width: 137.14286px;};
```

## 陣列 (List)

* 陣列的英文是 `Array`，而 Sass 只有 `List`，但用法上相同
* 陣列是用來存放 "沒有鍵名的變數"，表示只能透過 "索引" (1,2,3,4)取值，而不是 "鍵名"(hover,active,...) ，而一般程式語言都從 `0` 開始，但 List 從 `1` 開始，透過 nth() 使用 List

  ``` scss
  $color: red, blue, green;
  // in scss
  p {color: nth($color, 2)}
  // in css
  p {color: blue}
  ```

* 當然也可以位索引導入變數

  ``` scss
  $color: red, blue, green;
  // in scss
  $index: 2;
  p {color: nth($color, $index)}
  // in css
  p {color: blue}
  ```

## 物件 (Map)

* 陣列的英文是 `Object`，而在 Sass 中只有 `Map`，但用法相同
* `Map` 與 `List` 不同之處在於可以 "自訂鍵名"，也就是每個值的名稱
* Map 使用小括號 `( )` 包住資料，內層使用逗號 `,` 隔開資料，每筆資料都有獨立的鍵名 `key` 與值 `value`，每個鍵名需用引號 `""`、`''` 包住

  ``` scss
  $map: (
    'key1': value1,
    'key2': value2
  )
  ```

* `使用 map-get(map, key)` 將值取出

  ``` scss
  // in scss
  $theme: (
    'primary': blue,
      'secondary': red,
  )
  p {color: map-get($theme, primary)}

  // in css
  p {color: blue}
  ```

* Map 指令

  * `map-get(map, key)` : 取出指定的值

  * `map-merge(map1, map2)` : 合併 map

  * `map-remove(map, key)` : 移除指定的值

  * `map-keys(map)` : 列出 map 全部的 key

  * `map-values(map)` : 列出 map 全部的 value

  * `map-has-key(map, key)` : 確認 map 中是否有指定 key
