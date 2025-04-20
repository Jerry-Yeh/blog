---
title: JavaScript - ES6 - 縮寫
tags:
  - JavaScript
  - ES6
categories: JavaScript
abbrlink: 54708
date: 2019-10-22 14:30:13
thumbnail:
banner:
description:
---

ES6 提供了所多縮寫的方式，使程式碼更為簡潔

<!-- more -->

## 物件縮寫

以往物件要賦予到另一個物件的相同名稱屬性上時，需要寫成相同的 `屬性:物件`， ES6 開始可以不用重複撰寫，只要寫一次即可

``` js
let name = '草帽黑賊團';
const team = {
  船長: '魯夫',
  船醫: '喬巴'
}

// 以往寫法 
const newTeam = {
  name: name,
  team: team
}

// ES6 縮寫
const newTeam = {
  name,
  team
}
```

## 物件函式縮寫

當物件屬性的值是一個函式時，可以省略 `:function`，語意上不會有變化，並不會轉為箭頭函式

``` js
// 以往寫法
const team = {
  slogan: function (){
    console.log('我要成為海賊王');
  }
}

// ES6 縮寫
const team = {
  slogan (){
    console.log('我要成為海賊王');
  }
}
```

## 變數作為物件屬性

ES6 開始可以使用 "中括號" `[]` 來包住變數作為物件的屬性

``` js
let name = '海賊團名稱';
let value = '草海賊團';

let team = {
  [name]: value
}

// 會等於以下結果
let team = {
  海賊團名稱: '草帽海賊團'
}
```