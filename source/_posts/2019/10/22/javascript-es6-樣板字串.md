---
title: JavaScript - ES6 - 樣板字串
abbrlink: 30921
date: 2019-10-22 14:12:43
tags:
  - JavaScript 
  - ES6
categories: JavaScript
thumbnail:
banner:
description:
---

以往在組字串時，都是透過 引號 `'`、`"` 和 加號 `+` 來組合，換行也必須在結尾加上 反斜線 `\`，撰寫上容易混亂，閱讀也不夠自然，因此 ES6 新增了組字串的新方法，組起來也較為接近原生的 HTML

<!-- more -->

## ES6 字串基本寫法

ES6 新增的方法如下

* 使用 重音符 ` 來取代引號包住字串
* 使用 `${ }` 來包住 "變數" 或 "函式"，取代加號 `+` 來連接

  ``` html
  <ul class="list"></ul>

  <!-- after js -->
  <ul class="list">
    <h2>我是標題</h2>
    <img src="logo.png" alt="">
    <p>我是內容</p>
  </ul>
  ```

  ``` js
  const list = document.querySelector('.list');
  const title = '我是標題';
  const imgUrl = 'logo.png';
  function func(){
    return '我是內容';
  };

  // 傳統寫法
  list.innerHTML = '<li>\
    <h2>' + title + '</h2>\
    <img src="' + imgUrl + '" alt="">\
    <p>' + func() + '</p>\
  </li>';

  // ES6
  list.innerHTML = `
  <li>
    <h2>${title}</h2>
    <img src="${imgUrl}" alt="">
    <p>${func()}</p>
  </li>
  `;
  ```

  * 如果需要插入重音符 ` 或其他特殊字元，可以使用 "反斜線" \ 來進行轉義

  ``` js
  let text = `\`Hello\` World`;
  console.log(text);   // `Hello` World
  ```

## ${} 插入 JavaScript 原始碼

在 ES6 新增的 `${}` 還可以直接插入 JavaScript 原始碼 ; 如下範例要將陣列的中的值帶到字串中，以下是一般寫法

``` js
const people = [
  {
    name: '魯夫'
  },
  {
    name: '索隆'
  },
  {
    name: '娜美'
  }
]

let originUl = `
  <ul>
    <li> ${people[0].name} </li>
    <li> ${people[1].name} </li>
    <li> ${people[2].name} </li>
  </ul>
`
```

這裡我們透過 `map()` 將陣列做迴圈處理，並回傳處理後的結果，但處理後的結果因為原本是陣列，所以會用逗號 `,` 隔開，要再透過 `join()` 取消逗號 ,，而 `join()` 這個方法的預設是 `join(',')`，所以帶入空值 `join('')` 會取消逗號

``` js
let newUl = `
  <ul>
    ${people.map(person => `<li> ${person.name} </li>`).join('')}
  </ul>
`
```