---
title: JavaScript入門篇 - 流程判斷
tags: JavaScript
categories: JavaScript
abbrlink: 2be5d539
date: 2019-09-27 02:17:48
thumbnail:
banner:
description:
---

JS 用來控制流程的條件語法指的是，當指定條件為 `true`，就會執行後續指令，JS 條件語法分為以兩種 : `if...else` 和 `switch`

<!-- more -->

## if...else

顧名思義，如果怎樣，就執行某事，否則就執行另一件事，而其中的條件會透過 `ToBoolean` 轉換成 `true` 或 `false`

* 使用 小括號 `()` 包住 條件
* 使用 大括號 `{}` 包住 條件成立時要執行的事

``` js
if ("條件"){
  // 條件成立執行這件事
}
else {
  // 否則執行這件事
}
```

除了 `if` 和 `else`，還可以透過 `else if` 增加條件，增加條件可以不只一個，但為了維護與可讀性，建議不要過度增加條件

``` js
if ("條件1"){
  // 條件成立執行這件事
}
else if ("條件2"){
  // 以上條件都不成立就執行這件事
}
else if ("條件2"){
  // 以上條件都不成立就執行這件事
}
else {
  // 以上條件都不成立就執行這件事
}
```

## switch

`switch` 和 `if..else` 具有相同的功能，但當條件過多時，使用 `if...else` 會因為需要跑完每個條件而造成維護不易且效能較差

* 同樣使用 小括號 `()` 包住 條件 但不比較
* `case` 後方放要 與條件進行判斷的值
* 值後使用 冒號 :隔開 條件成立後要執行的指令
* 每個 `case` 最後 使用 `break;` 隔開其他的 `case`，如果沒有使用 b`reak;` 隔開，就會執行所有 `case` 的指令
* 如果以上所有 `case` 都不成立，就執行 `default` 中的指令

## if...else 與 switch 比較

``` js
var month = 1;

// if...else

if( Math.ceil( month/3 ) === 1 ){
  console.log('春天');
}
else if( Math.ceil( month/3 ) === 2 ){
  console.log('夏天');
}
else if( Math.ceil( month/3 ) === 3 ){
  console.log('秋天');
}
else if( Math.ceil( month/3 ) === 4 ){
  console.log('冬天');
}
else{
  console.log('月份錯誤');
}

// switch

switch ( Math.ceil(month / 3)){
  case 1:
    console.log("春天");
    break;
  case 2:
    console.log("夏天");
    break;
  case 3:
    console.log("秋天");
    break;
  case 4:
    console.log("冬天");
    break;
  default:
    console.log("月份錯誤");
    break;
}
```

如果沒有 `break`

``` js
var month = 1;

switch ( Math.ceil(month/3) ){
  case 1:
    console.log('春天');
  case 2:
    console.log('夏天');
  case 3:
    console.log('秋天');
  case 4:
    console.log('冬天');
  default:
    console.log('月份錯誤');
}

// in console
'春天'
'夏天'
'秋天'
'冬天'
'月份錯誤'
```

適當的使用 `break`

``` js
var month = 1;
switch (month){
  case 1:
  case 2:
  case 3:
    console.log('春天');
    break;
  case 4:
  case 5:
  case 6:
    console.log('夏天');
    break;
  case 7:
  case 8:
  case 9:
    console.log('秋天');
    break;
  case 10:
  case 11:
  case 12:
    console.log('冬天');
    break;
  default:
    console.log('月份錯誤');
}
```