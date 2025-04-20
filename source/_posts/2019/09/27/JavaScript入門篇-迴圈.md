---
title: JavaScript入門篇 - 迴圈
abbrlink: 25fb3458
date: 2019-09-27 02:23:17
tags: JavaScript
categories: JavaScript
thumbnail:
banner:
description:
---

迴圈指的是透過 "遞增" 或 "遞減" 來完成退出的條件，使我們不用反覆撰寫相同的程式，在 JS 中，常見的迴圈語法有 `for` 和 `while` 兩種

<!-- more -->

## for 迴圈

### for 基本寫法

* 使用 `for` 開頭
* 接著小括號 `()` 的內容分為三個部分， 初始值、條件、更新內容
* 當滿足條件，就會執行 大括號 `{}` 內的程式碼
* 當執行完 大括號 `{}` 內的程式碼，就會 更新值，如果符合條件，就會再次執行 大括號 `{}` 內的程式碼，如果不符合，就會結束並 跳出迴圈

### for 與 array 的寫法

在使用 `for` 取的陣列中的資料時，可以透過 `.length` 來取的陣列的長度，以此作為 `for` 迴圈執行次數的條件

``` js
// 撈出每個農場的農夫名字

var farms = [
  {
    farmer: '卡斯柏',
    field: 6
  },
  {
    farmer: '查理',
    field: 10
  }
  var farmsTotal = farms.length;
  for (var i =0; i < farmsTotal; i++){
    console.log('第' + (i+1) + '個農場主人是' + farms[i].farmer)
  };
]

// in console
第 1 個農場主人是 卡斯柏
地 2 個農場主人是 查理
```

### for 與 if 的寫法

透過在 `for` 中加入 `if` 可以為撈出的資料設定條件，而非撈出所有資料

``` js
// 撈出小雞數量超過 100 隻的農夫

var farms = [
  {
    farmer: '卡斯柏',
    chick: 50
  },
  {
    famrer: '約翰',
    chick: 120
  }
];

var farmsTotal = farms.length;

for (var i = 0; i < farmsTotal; i++){
  if (farms[i].chick > 100){
    console.log(farms[i].farmer + '的小雞超過 150 隻');
  }
};

// in console
約翰 的小雞超過 150 隻
```

### `for` 加總

透過 `+=` 我們可以不斷地將前面的項目累加上去

``` js
// 將所有香蕉數量加總

var farms = [
  {
    farmer: '卡斯柏',
    banana: 5000
  },
  {
    farmer: '查理',
    banana: 1000
  },
  {
    farmer: '約翰',
    banana: 3215
  }
]

var farmsTotal = farms.length;
var bananaTotal = 0;

for (var i = 0; i < farmsTotal; i++){
  bananaTotal += farms[i].banana;
};

console.log('今年的香蕉採收量為' + bananaTotal);

// in console
今年的香蕉採收量為 9215
```

## while 迴圈

`while` 和 `for` 在原理上大同小異，只是在寫法上有點不同

* 在外層宣告 "初始值"
* 使用 `while` 開頭
* 接著 小括號 `()` 包住 條件
* 再接著 大括號 `{}` 包住 符合條件時執行的內容
* 符合條件時執行的內容 後才寫 "更新值"

``` js
"初始值"

while ( "條件" ){
  // 符合條件時執行的內容
  "更新值"
};
```

## for 與 while 的差異

### 適合 `for` 的情境

因為 `for` 包含 `初始值`、`條件`、 `更新值` 三部分，能夠一目了然執行次數，所以更適合用在迴圈執行次數 "明確" 的狀態

### 適合 `while` 的情境

因為 `while` 只包含 `條件`，所以較適合用在迴圈執行次數 不明確 的狀態

## 無窮迴圈

當條件設定錯誤，不管如何都會符合條件時，就會無限執行內部程式碼，無法跳出迴圈，稱為 "無窮迴圈"

``` js
// 當 i 不管如何都會大於等於 0

// for
for (var i = 0; i >= 0; i++){
  console.log(i);
};

//while
var i = 0;
while ( i >= 0 ){
  console.log(i);
  i++;
};
```

## break 與 continue

### break : 會直接跳出迴圈
找到陣列中第一個 `非 0` 的數字並印出

``` js
var array = [0,0,7,0,9];
var i = 0;

while ( i < array.length ){
  if ( array[i] !== 0 ){
    break;
  };
  i++;
};
```

### continue: 只會跳過這次，然後繼續執行下一次迴圈
印出 1 ~ 10 的數字但跳過 3 的倍數

``` js
for (var i = 1; i <= 10; i++){
  if ( i % 3 === 0 ){
    continue;
  }
  console.log(i);
}
```


