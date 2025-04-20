---
title: Initialization
abbrlink: 4260
date: 2022-01-05 23:31:24
tags:
categories: 克服JS的奇怪部分
thumbnail:
banner:
description:
---

這堂課將介紹一個常見的情況 - 初始化 Initialization

<!-- more -->

當我們在開發時，有時會見到一段物件資料透過基本的方法 object literal、array literal 或 expression function 來建立，如下:

```js
var people = [
  {
    firstname: 'John',
    lastname: 'Doe',
    addresses: [
      '111 Main St.',
      '22 Third St.'
    ]
  },
  {
    firstname: 'Jane',
    lastname: 'Doe',
    addresses: [
      '333 Main St.',
      '444 Fifth St.'
    ],
    greet: function() {
      return 'Hello !';
    }
  }
];
```

這一個陣列可能包含許多資料，物件的資料又可能包含許多屬性與方法，但當我們看到這種情況時，其實不需要太緊張，因為這其實是非常直覺又簡潔的資料呈現方式，常用於在透過 API 取得資料前，先給定一些預設值來呈現。

## Error log

當我們在開發時，如果遇到以下 error log，常常是漏掉了分隔資料的逗號 `,`，導致資料結構出現非預期的呈現。

![](error-log.png)

## 資料來源

[Udemy-JavaScript: Understanding the Weird Parts](https://www.udemy.com/course/understand-javascript/)`
