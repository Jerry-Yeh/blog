---
title: JaveScript入門篇 - 陣列(Array)與物件(Object)
abbrlink: 63428deb
date: 2019-09-26 19:30:02
tags: JavaScript
categories: JavaScript
thumbnail:
banner:
description:
---

在前兩篇關於變數的文章中有提到，在 JavaScript 中，除了基本型別以外都可以歸類為物件型別，在這篇文章中就來介紹最基本的物件型別，陣列 `Array` 與物件 `Object`

<!-- more -->

## 物件 `Object`

* 物件可以是零至多個屬性 `property` 的集合
* 每個屬性 `property` 都有自己的值 `value`
* 值可以是基本型別、另一個物件 `object` 、陣列 `array` 或函數 `function`
* 使用大括號 `{ }` 建立起新的物件
* 屬性 property 使用冒號 `:` 隔開值 `value`，而值 `value` 結尾使用逗號 `,` 連接下一個屬性 `property`
* 屬性 `property` 可以透過 "點" `.` 來存取

  ``` js 
  var farm = {
    farmer: "卡斯伯",   // string
    dogs: ['張姆士', '龐的'],   // array(object)
    chicken: 15,   // number
    duck: 3,
    poultryTotal: function(){
      var num = farm.chicken + farm.duck;
      console.log(num);
    },
    "001": "Hello"
  }

  console.log(farm.farmer);   // 卡斯伯
  farm.poultryTotal();   // 18
  ```

* 屬性也可以透過中括號 `[ ]` 來存取，好處是遇到不合法的 JavaScript 的識別字 (如帶有空白的字串或是數字) 時，不會出現錯誤

  ```js
  farm.001;   // SyntaxError: Unexpected number
  farm["001"];   // "Hello"
  ```

* 屬性 `property` 可以透過 `=` "新增" 與 `delete` "刪除"

  ``` js
  var obj = { };

  obj.name = '123';   // 新增

  obj.name;   // '123'

  delete obj.name;   // 刪除

  obj.name;   // 刪除屬性後變成 undefined
  ```

### 判斷屬性是否存在
判斷屬性是存在最簡單的方式就是檢查該屬性是不是 `undefined`

``` js
var obj = {};
          
console.log( obj.name );   // undefined
```

但如果該屬性的值剛好是 `undefined`，此方法就無效，除了此方法，還有 `in` 運算子和 `hasOwnProperty` 方法

``` js
var obj = {
  name: 'Object'
};

// 透過 in 檢查屬性
console.log( 'name' in obj );     // true
console.log( 'value' in obj );    // false

// 透過 hasOwnProperty() 方法檢查
obj.hasOwnProperty('name');       // true
obj.hasOwnProperty('value');      // false
```

### 列舉物件 `(for ... in)`

另一個判斷物件內有什麼屬性的方法就是使用列舉 `(for ... in)`

``` js
var Person = {
  name: 'Jerry',
  age: 18,
  bwh: {
    strength: 30,
    agility: 31,
    intelligence: 32
  },
  single: true
}

for (prop in Person){
  console.log(Person[prop]);   // Jerry 18 {strength: 30, agility: 31, intelligence: 32} true
}
```

## 陣列 `Array`

* 陣列可以是零至多個元素的集合
* 陣列內可以是 原始的資料類型、其他陣列 `array`、函式`function` 或物件 `object`
* 使用中括號 `[ ]` 建立起新的陣列
* 每個元素以逗號 `,` 隔開
* 可以透過 `.length` 來取得陣列長度，而陣列長度是 可以被覆寫 的，如下原來的陣列長度為 3，透過 `a.length = 1` 將陣列長度改成 1 後，後面的元素就被移除了，之後再透過 `a.length = 3` 將陣列長度改回 3，後面被移除的元素也只會使用 `undefined` 來填補

  ``` js
  var a = ["apple", "boy", "cat"];
  a.length;    // 3

  a.length = 1;
  console.log(a);   // ["apple"]

  a.length = 3;
  console.log(a);   // ["apple", undefined, undefined]
  ```

* 陣列的索引是從 `0` 開始計算的

  ``` js
  array = ["a", "b", "c"];
  array[0] = "a";
  ```

* `.push('要新增的資料')` 新增資料

  透過 `.push()` 可以在陣列末端新增新元素

  ``` js 
  var array = ['a', 'b', 'c'];

  array.push('d');
  console.log(array);   // ['a', 'b', 'c', 'd']
  ```

* `.splice('從第幾筆資料開始','要刪除幾筆資料')` 刪除資料

  透過 `.splice()` 可以刪除陣列中指定資料以後的資料

  ``` js
  var array = ['a', 'b', 'c']

  array.splice('1','2');   // ['b', 'c']
  console.log(array);   // ['a']
  ```

* 可以直接指定改變元素

  ``` js
  var array = ['a', 'b', 'c'];

  array[1] = 'B';
  console.log(array);   // ['a', 'B', 'c']
  ```

* 除了 `.push` 外，陣列還有許多語法，可以參考以下連結

  [Array 語法](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array)

* 使用 `typeof` 來判斷陣列和物件都會顯示 `object`，我們可以使用 `.inArray` 來判斷此變數是陣列而非物件

  ``` js
  Array.isArray([]);            // true
  Array.isArray([1]);           // true
  Array.isArray(new Array());   // true

  Array.isArray();              // false
  Array.isArray({});            // false
  Array.isArray(null);          // false
  Array.isArray(undefined);     // false
  ```

## 陣列 + 物件

陣列和物件時常會混和著使用，陣列透過中括號 `[ ]`，而物件透過點 `.` 來撈出資料

``` js
var farms = [
  {
    dogs: ['張姆士', '龐的'],
    chick: 15,
  },
  {
    dogs: ['皮皮'],
    chick: 30
  }
];

console.log(farm[1].dogs[0]);   // '皮皮'
```

