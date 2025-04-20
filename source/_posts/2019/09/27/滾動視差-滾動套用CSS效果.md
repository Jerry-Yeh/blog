---
title: 滾動視差-滾動套用CSS效果
tags: jQuery
categories: jQuery
abbrlink: 1d3a5dfe
date: 2019-09-27 00:37:46
thumbnail:
banner:
description:
---

在這篇文章中，會透過 jQuery 來達到滾動網頁到指定位置時，套用特定的 CSS 效果

<!-- more -->

## 建立版型

首先，建立要動態套用 CSS 的版型，在這裡會做出讓 "三大重點" 中的項目在滾到到該區域時，由下而上浮現
**[範例版型](https://codepen.io/Jerry-Yeh/pen/vYBgeVv?editors=1010)**

## 隱藏內容

要做出動態浮現的效果時，我們會預設內容是隱藏的，但在這裡使用的是透明化 `opacity: 0`，而不是直接未顯示 `display: none`，因為在透明化的情況下才會 "保留元素空間"，避免版型走位

在這裡設定一個 `className` 並帶入元素中

``` css
.animated {
  opacity: 0;
}
```

``` html  
}
<div class="col-three-one animated">
  <div class="text-center m-y-2">
    <span class="fa fa-mobile fa-5x"></span>
  </div>
  <h3 class="text-center m-y-1">
    Fully Responsive
  </h3>
  <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Natus aut dolorem in autem veniam, nisi repellendus amet a eum laboriosam veritatis doloremque consectetur.</p>
</div>
```

## 效果製作

接著再新增一個 `className` 來製作呈現的效果，在這裡先增加不透明的效果 `opacity: 1`

``` css
.fadeIn {
  opacity: 1;
}
```

## jQuery 滾動

### 滾動方法

在這裡透過 `jQuery` 的滾動方法 `scroll` 來觸發滾動事件，並綁定在頂層物件 `window` 上

```js
$(document).ready(function (){
  $(window).scroll(function (){});
});
```

## 取得位置

如果要做出這個效果，就必須取得元素的座標位置，才得以在滾動到該位置時觸發效果

### 取得元素位置 offset 與 position

在 `jQuery` ，這兩種方法都可以取得元素的座標位置，可以先透過 `consle.log` 來查看取得的座標

* `top`: 取得指定元素的 `y` 軸 座標
* `left`: 取得指定元素的 `x` 軸 座標

``` js 
var thisPos = $(this).offset().top;
var thisPos = $(this).position().left;
```

**`offset` 與 `position` 的差異**
這兩種方法的差異在於 "相對位置"

* `offset`: 以最高級元素 `document` 也就是整個網站來做為相對位置
* `position`: 會以指定元素網上尋找，直到找到第一個含 `position: relative` 屬性的元素來取得相對座標

### 取得網頁高度

取得網頁高度有助於在觸發事件的位置上更靈活，而不會在滾動到該區域的最上方才會觸發

``` js
var windowHeight = $(window).height();
```

## 套用座標位置

在了解滾動方法並取得座標位置後，就可以開始設定條件來呈現 CSS 效果

**取得元素**
透過方法 `each()` 來取得所有包含該 `className` 的元素

``` js
$('.animated').each(function (){});
```

**取得元素座標**
取得元素後，再透過上述提到的方法來取的座標
``` js
$('.animated').each(function (){
  var thisPos = $(this).offset().top;
});
```

**設定條件**
在這裡設定如果 "網頁座標" 加上 "網頁高度" 大於等於 "元素座標"，就加上要套用的 CSS 效果

``` js
$(window).scroll(function (){
  var scrollPos = $(window).scrollTop();
  var windowHeight = $(window).height();

  $('.animated').each(function (){
    var thisPos = $(this).offset().top;
    if ((windowHeight + scrollPos) >= thisPos){
      $(this).addClass('fadeIn');
    };
  });
});
```

如此一來，就可以在滾動到特定元素時，套用指定的 CSS 效果囉

## 資料參考

[六角學院課程 - 一變應萬變的響應式網頁設計](https://www.udemy.com/course/responsive/)
