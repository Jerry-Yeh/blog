---
title: JavaScript入門篇 - 運算式(Expression)與運算子(Operator)
tags: JavaScript
categories: JavaScript
abbrlink: d8cdf04e
date: 2019-09-27 00:55:59
thumbnail:
banner:
description:
---

在 JS 語法可以分為兩大類，敘述句 `Statement` 與 運算式 `Expression`

<!-- more -->

* 敘述句 `Statement` :

  簡單來說，敘述句就是執行某個動作，如變數的宣告、賦值、迴圈或 `if` 判斷式等，都可以被歸類於此

  ``` js
  var foo;
  ```

* 運算式 `Expression` :

  運算式最大的特性是會產生一個 值，如 `function` 的參數 `arguments`，或是透過等號 `=` 賦值時，在等號 `=` 右側的部分都屬於運算式

  ``` js
  var a = 10 * 10;
  ```

  如上，在等號 = 右側的 10 * 10 就是屬於運算式

## 運算子的分類

運算子的類型很多，常見的可以分為以下幾類，其餘可以操考 [MDN 運算式與運算子](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Expressions_and_Operators)

* 算術運算子 (Arithmetic Operator)
* 賦值運算子 (Assignment Operator)
* 比較運算子 (Comparison Operator)
* 邏輯運算子 (Logical Operator)
* 逗號運算子 (Comma Operator)
* 三元(條件)運算子

## 算術運算子 (Arithmetic Operator)

算術運算子包括大家熟知的 四則運算(加、減、乘、除)，但運算後的結果常常會出現一些特例，以下一一介紹

[加號 `+`](#加號)
[減號 `-`](#減號)
[乘號 `*`](#乘號)
[除號 `/`](#除號)
[取餘數 `%`](#取餘數)
[遞增 `++` 與 遞減 `--`](#遞增-與-遞減)

### 加號 `+`

* 單純數字的相加

  ``` js
  var a = 1 + 2;
  console.log(a);    // 3
  ```

* 特殊字無限大 `Infinity`、負無限大 `-Infinity` 和 `Not a Number NaN`

  ``` js
  Infinity + Infinity      // Infinity
  -Infinity + -Infinity    // -Infinity
  -Infinity + Infinity     // NaN
  ```

  而只要加號 `+` 兩側其中一個是 `NaN`，結果就會是 `NaN`

  ``` js
  10 + NaN            // NaN
  Infinity + NaN      // NaN
  -Infinity + NaN     // NaN
  ```

* 如果加號 `+` 兩側其中一個是 字串 `string`，另一端就會 "自動轉型" 為 "字串"

  ``` js
  // 數字 + 字串 
  100 + "100"      // "100100"
  100 + "ABC"      // "100ABC"

  // 字串 + 字串
  "ABC" + "XYZ"    // "ABCXYZ"

  // 布林 + 字串
  true + "abc"     // "trueabc"

  // 數字 + 物件
  100 + {}         // "100[object Object]"

  // 數字 + undefined，undefined 會被轉成數字 NaN
  123 + undefined  // NaN
  "abc" + undefined    // "abcundefined"

  // 數字 + null，null 會被轉成數字 0
  123 + null  // 123
  "abc" + null    // "abcnull"
  ```

* 由於運算式是 "由左而右" 且 "先乘除後加減" 做運算，所以如果要讓數字先做運算，可以使用小括號 `()` 將數字運算包起來

  ``` js
  var str1 = "10 加 100 的結果是 " + 10 + 100;   // "10 加 100 的結果是 10100"
      
  var str2 = "10 加 100 的結果是 " + (10 + 100);   // "10 加 100 的結果是 110"
  ```

### 減號 `-`

* 單純數字相減

  ``` js
  var a = 10 - 3;
  console.log(a);   // 7
  ```

* 特殊的數字，如無限大 `Infinity`、負無限大 `-Infinity` 和 `Not a Number NaN`

  ``` js
  Infinity - Infinity   // NaN
  -Infinity - -Infinity   // NaN

  -Infinity - Infinity    // -Infinity
  Infinity - -Infinity    // Infinity
  ```

  如果減號 `-` 兩側其中之一是 `NaN`，那結果就會是 `NaN`

* 與加號 `+` 不同的地方是如果減號 `-` 兩側其中之一不是數字，會透過 `Number()` 嘗試將數值轉為 "數字"

  * 不含數字的字串、`undefined` 和 物件 `object` 會被轉型成 `NaN`
  * 含數字的字串 會被轉型為 數字
  * `true`、會被轉型為 `1`
  * `false`、`null` 會被轉型為 `0`

    ``` js
    // 數字 - 字串
    100 - "50"    // 50
    100 - "abc"   // NaN

    // 數字 - 布林
    100 - false   // 100
    100 - true    // 99

    // 數字 - undefined
    100 - undefined   // NaN

    // 數字 - null
    100 - null   // 100

    // 數字 - 物件
    100 - {}   // NaN
    ```

### 乘號 `*`

* 單純數字的運算

  ``` js
  var a = 10 * 10;
  console.log(a);   // 100
  ```

* 超出數字的範圍由就看結果是正數或負數來決定是 `Infinity` 或 `-Infinity`
* 如果乘號兩側其中之一出現 `NaN`，那結果就會是 `NaN`， `Infinity * 0` 也是 `NaN`
* 如果乘號兩側其中之一出現非數字的型別，會和減號 `-` 一樣使用 `Number()` 嘗試轉型為 "數字"

  ``` js
  // 數字 * 字串
  100 * "10"   // 1000
  100 * "abc"   // NaN

  // 數字 * 布林
  100 * true   // 100
  100 * false  // 0

  // 數字 * undefined
  100 * undefined   // NaN

  //數字 * null
  100 * null   // 0

  // 數字 * 物件
  100 * {}   // NaN
  ```

### 除號 `/`

* 單純數字的運算

``` js
var a = 100 / 10
console.log(a);   // 10
```

* "被除數" 為 `0` 的情況下

  * 除數為 "正數"，結果為 `Infinity`
  * 除數為 "負數"，結果為 `-Infinity`
  * 除數為 `0`，結果為 `NaN`

* 和乘號 `*` 相同，如果除號 `/` 兩側其中之一為數字以外的型別，會透過 `Number()` 嘗試轉型為數字

### 取餘數 `%`

* 單純數字的運算

  ``` js
  var a = 100 % 33
  console.log(a);   // 1 
  ```

* 如果 "被除數" 為 `Infinity` 或 `-Infinity`，取餘數的結果為 `NaN`

  ``` js
  Infinity % 0   // NaN
  Infinity % 100   // NaN
  Infinity % Infinity   // NaN
  Infinity % -Infinity   // NaN
  ```

* 如果 "被除數" 為 "一般值"， "除數" 為 `Infinity`，結果為 "被除數"

  ``` js
  100 % Infinity   // 100
  0 % Infinity   // 0
  ```

* 如果 "被除數" 為 "一般值"， "除數" 為 `0`，結果為 `NaN`

  ``` js
  100 % 0   // NaN
  0 % 0   // NaN
  ```

* 和除號 `/` 相同，如果取餘 `%` 兩側其中之一為數字以外的型別，會透過 `Number()` 嘗試轉型為數字

### 遞增 `++` 與 遞減 `--`

* 如同字面上的意思，遇到 遞增 `++` 就會加一，可以看成是 `a = a + 1` ，遇到 遞減 `--` 就會減一，可以看成是 `a = a - 1`

  ``` js
  var a = 10;
      
  a++;
  console.log(a);   // 11

  a--;
  console.log(a);   // 10
  ```

* 位置放在變數前後是有差別的，將 遞增 `++` 放在 前面，回傳的會是 "原始數值" ，放在 後面，回傳的會是 `+1` 後的結果，但事後再將兩者印出，都會出現 `+1` 後的結果，同理 遞減 `--` 也是

  ``` js
  var a = 10;
      
  a++;
  console.log(a);   // 11

  a--;
  console.log(a);   // 10
  ```

## 賦值運算子 (Assignment Operator)

* 賦值運算子透過 等號 `=` 來將左側的值指派到右側，如下將右側的運算結果指派到左側的變數 `a`

  ``` js
  var a = 10 * 10;
  ```

* 除了單純等號 `=`，還可以結合算術運算子

  ``` js
  a += b;   // a = a + b
  a -= b;   // a = a - b
  a *= b;   // a = a * b
  a /= b;   // a = a / b
  a %= b;   // a = a % b
  ``` 

## 比較運算子 (Comparison Operators)

比較運算子是用來比較運算子兩側的值，可能是 `純值`、`物件`、`運算式` 或 `函式回傳` 的結果，比較後會得到 `true` 或 `false`

* ### 賦值 : 一個等號 `=`

  一個等號 `=` 的意思是將等號 `=` 右側的值 指定、賦值 到 等號 `=` 左側，為 賦值運算子

  ``` js
  var a = 10;
  ```

* ### 相等 : 兩個等號 `==`

  兩個等號 `==` 的意思是為左右兩側的資料進行比較，但 不同型別的值會 "自動轉型" 後再做比較

  ``` js
  var a = 10;
  var b = "10";

  console.log( a == 10 );   // true
  console.log( a == b );   // true
  console.log( a == 100  );   // false 
  ```

* ### 全等 : 三個等號 `===`

  三個等號 `===` 也是為左右兩側的值進行比較，但 "不同型別的值" 不會自動轉型

  ``` js
  var a = 10;
  var b = "10";

  console.log( a == b );   // true
  console.log( a === b );   // false
  ```

* ### 不等於 `!=` 與 `!==`

  等號 `=` 前面加上 驚嘆號 `!` 即為 "不等於"，而不等於分成兩種，一個等號 `!=` "不會自動轉型" ，兩個等號 `!==` "會自動轉型"

  ``` js
  var a = 10;
  ```

* ### 大於 `>` 與 小於 `<`

  除了判斷是否相等，還可以比較大小，在後方加上 等號 `=` 就會包含等於，大於等於 `>=` 和 小於等於 `<=` ，如遇到不同型別就會 "自動轉型"

  * 如果兩者都是 "數字" 則單純就字面大小比

    ``` js
    var a = 10;
    var b = 11;
    var c = 10;

    console.log( a > b );   // true
    console.log( b > a );   // false
    console.log( c > a );   // false
    console.log( a >= c );  // true
    ```

  * 如果其中之一不是數字，則會嘗試將其轉型成 "數字" 再做比較

    ``` js
    var a = 10;   // number
    var b = "9";   // string

    console.log( a > b );   // true
    ```

  * 如果兩者都是字串，則會依照字母的順序來做比較，如果有多個字母，則會比較到第一個不同的字母為止

    ``` js
    console.log( "a" > "b" );   // false
    console.log( "b" > "a" );   // true
    console.log( "abd" > "abc" );   // true
    ```

  * 如果其中之一是 布林 `boolean`，`true` 會被轉型為 `1`，`false` 會被轉型為 `0`

    ``` js
    console.log( 10 > true );   // true
    console.log( true > false );   // true
    ```

  * 如果其中之一是物件 `object`，會先透過 `valueOf()` 求得對應的數值，如果沒有 `valueOf()` 的方法，就會透過 `toString()` 轉型再做比較

## 邏輯運算子 (Logical Operator)

邏輯運算式包含三種，and `&&`、or `||`、not `!`，通常會在 `if 判斷式 `和布林 `boolean` 搭配使用

* ### And `&&`

  當 and `&&` 的兩側都是 `true` 結果才會是 `true`，否則就是 `false`，即 全部條件都必須成立才算成立

  ``` js
  var a = 10;
  var b = true;

  console.log( a > 0 && b = true );   // true
  console.log( a < 0 && b = true );   // false
  ```

* ### Or `||`

  當 or `||` 的兩側其中之一是 `true` 結果就會是 `true`，除非兩側都是 `false` 才會是 `false`，即 滿足其中一個條件就會成立

  ``` js
  var a = 10;
  var b = true;
                    
  console.log( a > 0 || b = true );   // true
  console.log( a < 0 || b = true );   // true
  ```

* ### Not `!`

  not `!` 代表否定、反向的意思，如果是 `! true`，就是 `false` 的意思，兩個驚嘆號 `!!` ，利用雙重否定可以取代 `Boolean()` 來判斷某數值 `Boolean` 轉換後的結果

  ``` js
  var a = 10;

  console.log( a = 0 );   // false
  console.log( a != 0 );   // true
  ```

* ### 布林 Boolean 的型別轉換

  * 透過 Boolean() 會轉換成 false 的值

    * undefined
    * null
    * 0
    * NaN
    * 空字串 "" 、''

  * 其他的值透過 Boolean 轉型後都會是 true

* ### 邏輯運算子的判斷

  透過邏輯運算子 `&&`、`||` 做判斷時，會先對左側進行驗證

  * 如果是 `Boolean` 類型就會做後續判斷，如果不是就會透過 `Boolean()` 轉型
  * 以 and `&&` 來說，如果第一個值為 `true`，則回傳第二個值，否則回傳第一個
  * 以 or `||` 來說，如果第一個值為 `true`，則回傳第一個值，否則回傳第二個

    ``` js
    var a = 123;
    var b = "abc";
    var c = null;

    console.log( a && b );   // "abc"
    console.log( a || b );   // 123

    console.log( c && a );   // null
    console.log( c || b );   // "abc"
    console.log( c || a );   // 123
    ```

## 逗號運算子 (Comma Operator)

逗號運算子 `,` 主要是分隔運算式中的多組運算式，常使用在 `for 迴圈` 中

``` js
for (var i = 0, j = 10; i < 10; i++, j++){
  k = i + j;
};
``` 

也可以使用在 "變數的宣告"，但要注意的是，以下兩種宣告是 "不相同" 的

``` js
var a = 10, b = 10;
var a = b = 10;
```

因為變數 `b`，其實沒有宣告，只有賦值，所以是全域變數

``` js
var a = b;
b = 10;

function (){
  var a = b = 10;
  console.log( a, b );   // 10, 10
};

// 離開作用範圍後
console.log(a, b);   // undefined, 10
```

## 三元運算子

三元運算子以 問號 `?` 和 冒號 `:` 所組成，如果條件為 `true`，回傳數值 `1`，否則回傳數值 `2`

``` js
"(條件)" ? "數值1" : "數值2"

// 如果年紀大於等於 "18"，為"成人"，否則為"小孩"
var status = (age >= 18) ? "成人" : "小孩";
```