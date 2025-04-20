---
title: JavaScript - ES6 - class 建構式語法糖
tags:
  - ES6
  - JavaScript
categories: JavaScript
abbrlink: 6199
date: 2019-10-23 11:03:43
thumbnail:
banner:
description:
---

JavaScript 與其他物件導向的語言不太一樣，是以 "原型" `prototype` 為基礎的物件導向，ES6 新增了 `class` 這個語法糖 `syntactical sugar`，使得原型的建構更簡潔好閱讀

<!-- more -->

以下列出 `class` 與原先建構原型的差異

## class 用法

* `function` 改成 `class`
* 初始化的物件屬性都寫在 `constructor()` 內，一個 `class` 只能有一個 `constructor`
* 原型的 "方法" 也可以一併寫在 `class` 內

以下先列出 ES6 以前的寫法

``` js
function strawHat(name, position, devilFruit){
  this.name = name;
  this.position = position;
  this.devilFruit = devilFruit || false;
};

// 原型方法
strawHat.prototype.capability = function (){
  if (this.devilFruit){
    console.log(`${this.name} 是能力者`);
  }else {
    console.log(`${this.name} 不是能力者`);
  }
};

strawHat.prototype.defeat = function (person){
  console.log(`${this.name} k.o ${person}`);
}

var Luffy = new strawHat('魯夫', '船長', true);
var zoro = new strawHat('索隆', '戰鬥員', false);

Luffy.capability();   // 魯夫 是能力者
zoro.defeat('卡古');   // 索隆 k.o 卡古
```

將上述範例換成 ES6 寫法

``` js
class strawHat {
  constructor (name, position, devilFruit){
    this.name = name;
    this.position = position;
    this.devilFruit = devilFruit || false;
  }

  capability (){
    if (this.devilFruit){
      console.log(`${this.name} 是能力者`);
    }else {
      console.log(`${this.name} 不是能力者`);
    }
  }

  defeat (person){
    console.log(`${this.name} k.o ${person}`);
  }
}
```

## 靜態方法 static

靜態方法 `static` 是原型專屬的方法，不會繼承到新物件上

``` js
class strawHat {
  constructor (name, position, devilFruit){
    this.name = name;
    this.position = position;
    this.devilFruit = devilFruit || false;
  }

  capability (){
    if (this.devilFruit){
      console.log(`${this.name} 是能力者`);
    }else {
      console.log(`${this.name} 不是能力者`);
    }
  }

  static solgan  (){
    console.log(`我要成為海賊王 !`);
  };
}

zoro.solgan();   // Error
strawHat.solgan();  // 我要成為海賊王 !
```

## 屬性的 get & set 方法

在 ES6 的 `class` 也可以一併將屬性的 讀入 `set` 和 "傳出" `get`

* `set` : 只能傳入一個參數，這個參數就是讀入的屬性值，通常都會加上下底線 `_` 將屬性值進行 "封裝"
* `get` : 用來回傳 `set` 封裝好的屬性值

所以如果只有 `get` 就無法將屬性值修改，而只有 `set` 則是修改後無法回傳

``` js
class Person {
  constructor (name, position){
    this.name = name;
    this.position = position;
  }

  get name (){
    return this._name;
  }

  set name(name){
    this._name = `我是 ${name}`
  }
}

let zoro = new Person;

zoro.name = '索隆';
console.log(p.name);   // 我是索隆
```

## extends 擴增原型

當我們以這個物件為原型建立了許多新物件，其中有新物件想要修改屬性或是新增方法，但又不希望影響到其他的新物件，這時候我們就可以透過 `extends` 來擴增新的原型，有點像是 `git` 建立分支的概念

* `extends` : 用來指定要繼承的原型
* `super` : 要沿用的屬性可以放在這裡

``` js
class slogan extends Person {
  constructor (name, position){
    super(name);
    this.position = `位置 : ${position}`;
  }

  solgan (){
    console.log('我要成為海賊王');
  }
}

let zoro = new person('索隆');
let Luffy = new slogan('魯夫', '船長');
Luffy.solgan();   // 我要成為海賊王
zoro.solgan();   // Error
```

