---
title: Scss mixin
abbrlink: 4fef1f83
date: 2019-09-24 23:21:18
tags: Scss
categories: Scss
thumbnail:
banner:
description:
---

透過 `@mixin` 可以省去重複撰寫相同的程式碼，並將 CSS 技巧儲存成工具包，快速取用，避免往後用到時還要從舊資料找方法

<!-- more -->

## @mixin 用法

以指令 `@mixin` 進行宣告，並以 `@include` 呼叫載入，以下為 "圖片取代文字" 的 CSS 技巧

``` scss 
//in Scss
@mixin text-hide {
  text-indent: 101%;
  overflow: hidden;
  white-space: nowrap;
}
.brand {@include text-hide;}

// in css
.brand {
  text-indent: 101%;
  overflow: hidden;
  white-space: nowrap;
}
```

## 參數

`@mixin` 還可以導入參數並給予預設值，如果沒有給值就會自動導入預設值

``` scss 
// in scss
@mixin bg ($bgc:red, $width:200px){
  background-color: $bgc;
  width: $width;
}
.box {@include bg}
.box2 {@include bg(blue, 300px)}

// in css
.box {
  background-color: red;
  width: 200px;
}
.box2 {
  background-color: blue;
  width: 300px;
}
```

##  #{ } 字串內插

* Sass 的字串內插是將一個字串型態的變數放在兩個字串之間
* 對於 Sass 的組合來說，字串內插使我們能在選取項中重複使用 相同的 "字串變數" 和 "檔案名稱"

  ``` scss
  // in scss
  @mixin bgi ($file){background-image: url(#($file).png)};
  .box {@include bgi(test)};

  // in css
  .box {background-image:url(test.png)};
  ```

* 如果要將變數帶入 `class` 也必須使用此符號包住變數

``` scss
/* in scss */
$name :primary;
.bg-#{$name} {...}

/* in css */
.bg-primary {...}
```

## @content 傳遞內容

`@content` 的功能在於傳遞內容到 `@mixin` 中

* 用在 RWD 斷點設計中，為每個斷點傳遞不同的內容

  ``` scss
  // in Scss
  @mixin mobile {
    @media (max-width: 768px){
      @content
    }
  };
  .header {
    width: 500px;
    @mixin mobile {
      width: 300px;
    }
  }

  // in css
  .header {width: 500px};

  @media (max-width: 768px){
    .header {width: 300px}
  };
  ```

* 用在狀態樣式的表示

  ``` scss
  // in scss
  // 連結樣式
  @mixin link {
    &:link, &:visited {
      @content;
    }
  }
  // 被點擊後樣式
  @mixin link-hover {
    &:hover, &focus, &:active, &.active {
      @content;
    }
  }
  .box {
    width: 50px;
    @include link {color:red};
    @include link-hover {color: blue};
  }

  // in css
  .box {width: 50px};
  .box:link, .box:visited {color: red};
  .box:hover, .box:focus, .box:active, .box.active {color: blue};
  ```



