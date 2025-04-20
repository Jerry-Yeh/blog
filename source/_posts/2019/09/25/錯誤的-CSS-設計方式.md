---
title: 錯誤的CSS設計方式
tags: CSS
categories: CSS
abbrlink: 1185305d
date: 2019-09-25 01:31:57
thumbnail:
banner:
description:
---

在這篇文章中紀錄一下在撰寫 CSS 時，常見的錯誤方式與不利於後續開發的壞習慣

<!-- more -->

## CSS 範圍性常見錯誤

如果是滿足大部分的樣式盡量寫在外層，其餘的差異再由內層覆蓋，避免重複撰寫相同樣式

``` html
<head>
  <style>
    /* 滿足大部分樣式 */
    body {font-family: serif}
    .wrap {font-size: 16px}
    /* 其餘差異 */
    .footer {font-size: 12px}
  </style>
</head>
<body>
  <div class="wrap">
    <div class="header">
      <p></p>
    </div>
    <div class="footer">
      <p></p>
    </div>
  </div>
</body>
```

## 請勿將高度寫死

如果將外層高度寫死，當內層高度超過時就無法自適應延伸而破圖，此問題在 `RWD` 尤為明顯，盡量使用 `margin` 和 `padding` 產生相對空間

## 請勿使用 ID 綁定網頁樣式

ID 具備 "錨點功能" ，可以定位網頁位置，也具備 "唯一性" ，如果使用 ID 模組化，後續開發就無法重複使用，且權重較高，新增樣式時難以覆蓋

## 解決 JS 與 CSS 功能互相干擾

將 JS 效果的 class 與樣式分離，好處在於修改 CSS 樣式時不用連同 JS 一併更改，瀏覽 HTML 時，也可以清楚找到有 JS 效果的位置，可以使用前贅詞 `js-` 區分

``` html  
<input type="button" class="btn btn-primary js-hello" value="hello">
```