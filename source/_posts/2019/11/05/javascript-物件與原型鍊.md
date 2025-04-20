---
title: JavaScript 物件與原型鍊
tags: JavaScript
categories: JavaScript
abbrlink: 49319
date: 2019-11-05 12:34:42
thumbnail:
banner:
description:
---

<!-- @format -->

JavaScript 是一門 "物件導向" 的程式語言，但物件彼此是透過 "原型" 來繼承屬性，在介紹原型鍊之前，會先透過 "串聯" `Cascade` 與物件來了解背後的觀念

<!-- more -->

## Cascade 串聯

在 javascript 的函式 function 中，如果沒有設定 `return` 的值，那麼預設就會回傳 `undefined`，而 `function` 中的 `this` 會指向 "呼叫這個函式的物件" ，如果將 `return` 的值設為 `this`，就可以達到串聯 `Cascade` 的方式，也是 `jQuery` 可以串聯許多方法背後的觀念

### 一般寫法

以下為一個運算的範例

```js
var count = function (num) {
  this.num = num;

  this.add = function (newNum) {
    this.num += newNum;
  };

  this.sub = function (newNum) {
    this.num -= newNum;
  };
};
```

接著透過 `new` 來新增物件，要注意的是，如果沒有 `new`，該新增變數的值就會是函式 `count` 回傳的值，也就是預設的 `undefined`

```js
var a = new count(100);
var b = count(100);

console.log(typeof a); // object
console.log(typeof b); // undefinde
```

建立物件後，就可以呼叫物件內的函式，又稱為方法 `Method`

```js
a.add(100);
console.log(a.num); // 200

a.sub(50);
console.log(a.num); // 150
```

### Cascade 的方式

一般的寫法如上，在呼叫物件中的多個方法 `Method` 時，需要個別呼叫 ; 如果將原物件中的方法都加上 `return this`，回傳 `this`

```js
var count = function (num){
  this num = num;

  this.add = function(newNum){
    this.num += newNum;
    return this;
  }

  this.sub = function(newNum){
    this.num -= newNum;
    return this;
  }
}
```

這時候就會發現可以將多個方法 `Method` 在一行程式碼一起呼叫

```js
var a = new count(100);

a.add(100).sub(50);
console.log(a.num); // 150
```

拆開來看的話，`a.add(100)` 在運算完後會 `return this`，其中 this 就是屬性 `num` 變成 `200` 的 `a` 物件，而不是預設的 `undefined`，所以就可以接著呼叫下一個方法 `Method`

這就是 `jQuery` 常見的方法鍊

```js
$("div").addClass("is-active").removeClass("is-hide");
```

## 物件 Object

### 型別 Type

在 JavaScript 中，主要區分為兩大型別，基本型別 `Primitives` 與 物件型別 `Object`，除了基本型別外，都可以被歸類為物件型別

#### 基本型別

- 字串 `string`
- 數字 `number`
- 布林 `boolean`
- 空值 `null`
- 未定義 `undefined`
- ES6 新增的 `stymbol`

### 自訂物件

在 JavaScript 中，有兩種方式可以自訂物件

- 使用 `new` 關鍵字來建立

  ```js
  var person = new Object();
  preson.name = "Jerry";
  ```

- 使用大括號 `{ }` 直接建立新物件

  ```js
  var person = {
    name: "Jerry",
  };
  ```

### 建構式 Constructor

JavaScript 不像其他物件導向的語言具有 `class` 語法，但可以透過建構式來建立各種對應的物件，要注意的是建構式是使用 "函式" `function` 來建立物件樣板，不能使用 `ES6` 的 "箭頭函式"

- `function` 中的 `this` 代表物件的屬性
- 可以透過參數來傳入數值
- 使用關鍵字 `new` 來建立新物件

  ```js
  function Person(name, age, gender) {
    this.name = name;
    this.age = age;
    this.gender = gender;

    this.greeting = function () {
      console.log("Hello!" + this.name);
    };
  }

  var Jerry = new Person("Jerry", 24, "male");
  Jerry.greeting(); // 'Hello! Jerry'

  var Tom = new Person("Tom", 18, "female");
  Tom.greeting(); // 'Hello! Tom'
  ```

  將建構式拆開來看，其實是先建立一個空物件 `{ }`，再透過 `call` 方法將 `this` 指向這個物件，並傳入參數

  ```js
  var Jerry = {};
  Person.call(Jerry, "Jerry", 24, "male");
  ```

  透過建構式建立的物件，還是可以透過 `.` 來存取屬性

  ```js
  console.log(Jerry.age); // 24
  Jerry.age = 18;
  console.log(Jerry.age); // 18
  ```

### 屬性描述器 Property descriptor

從 ES5 開始，我們可以透過新的物件模型來靠至物件屬性的存取、刪除、列舉等。這些特殊的屬性就稱為 "屬性描述器" `Property descriptor`

屬性描述器分為六種 :

- `value` : 屬性的值
- `writable` : 定義屬性是否可以改變，如果是 `false` 就是唯讀屬性
- `enumerable` : 定義屬性是否可以透過 `for-in` 語法來迭代
- `configurable` : 定義屬性是否可以被刪除、或修改屬性的 `writable`、`enumerable` 和 `configurable` 設定
- `get` : 屬性的 `getter function`，預設為 `undefined`
- `set` : 屬性的 `setter function`，預設為 `undefined`

這些 "屬性描述器" 都必須透過 ES5 的 `Object.defineProperty` 來處理

#### 定義屬性 `Object.defineProperty` 與 查詢屬性描述 `Object.getOwnPropertyDescriptor`

- `Object.defineProperty(object, property, descriptor)` : 用來定義物件屬性的描述

  ```js
  var Person = {};
  Object.defineProperty(Person, "name", { value: "Jerry" });

  // resault
  var Person = {
    name: "Jerry",
  };
  ```

  當然也可以一次定義多個屬性描述

  ```js
  Object.definedProperty(person, "name", {
    value: "Jerry",
    writable: true,
    enumerable: true,
    configurable: true,
  });
  ```

- `Object.getOwnPropertyDescriptor(Object, Property)` : 用來檢查物件屬性描述器的狀態

  ```js
  Object.getOwnPropertyDescriptor(Person, "name");
  // {value: 'Jerry', writable: false, enumerable: false, configuarable: false}
  ```

要注意的是，使用兩種物件屬性的定義方式在 `writable`、`enumerable` 和 `configurable` 這三種屬性描述器上會有不同的預設值，使用 `Object.definedProperty` 定義的物件屬性的預設值會是 `false`，直接定義物件的預設值則會是 `true`

```js
var person = {};
Object.defineProperty(person, "name", {
  value: "Jerry",
});
person.name = "Tom";
console.log(person.name); // Jerry
console.log(Object.getOwnPropertyDescriptor(person, "name"));
// {value: "Jerry", writable: false, enumerable: false, configurable: false}

var Person = {
  name: "Jerry",
};
Person.name = "Tom";
console.log(Person.name); // Tom
console.log(Object.getOwnPropertyDescriptor(Person, "name"));
// {value: "Jerry", writable: true, enumerable: true, configurable: true}
```

#### 屬性描述器 get 與 set

我們可以透過 屬性描述器 `get` 與 `set` 來追蹤物件屬性的更新，以下範例透過屬性 `_name_` 來作為 `name` 屬性的封裝，物件屬性更新時會執行 `set`，讀取物件屬性則會執行 `get`，且在自定義 `get` 和 `set` 後，就無法再透過 `Object.defineProperty` 來更改屬性描述器，並將其他屬性描述器設定為 `false`

```js
var person = {};

Object.defineProperty(person, "name", {
  get: function () {
    console.log("get");
    return this._name_;
  },
  set: function (name) {
    console.log("set");
    this._name_ = name;
  },
});
person.name = "Jerry"; // set

// Uncaught TypeError
Object.defineProperty(person, "name", {
  value: "Tom",
  writable: false,
  enumerable: true,
  configurable: true,
});

console.log(person.name);
// get
// Jerry
console.log(Object.getOwnPropertyDescriptor(person, "name"));
// {get: ƒ, set: ƒ, enumerable: false, configurable: false}
```

## 基本型別包裹器 Primitive Wrapper

### 物件型別

上文提到，除了基本型別以外都可以歸類在物件型別，而在物件型別中，又可以細分為以下幾種 "建構器" `Constructor`

- String()
- Number()
- Boolean()
- Array()
- Object
- Function
- RegExp
- Date
- Error
- Symbol

以上這些建構器都可以透過關鍵字 `new` 來建立對應的物件

### 基本型別包裹器 Primitive Wrapper

物件中的 "變數" 稱為 "屬性"，而物件中的 "函式" 稱為 "方法"，但在基本型別中卻有 "屬性" 和 "方法" 可以呼叫

```js
var str = "Hello";

console.log(str.length); // 5
console.log(str.toUpperCase()); // 'HELLO'
```

因為在存取字串 `String`、數字 `Number` 和布林 `Boolean` 的那一刻會被 "自動轉型" 為該類別的物件，這就是基本型別包裹器 `Primitive Wrapper`

當我們在呼叫基本型別的屬性和方法時，背後的原理是透過對應的物件建構器包裝成 "物件"，再回傳對應的屬性後銷毀並回復成基本型別

```js
var str = new String("Hello");
str.length;

str = null;
str = "Hello";
```

基本型別並沒有 "屬性" 和 "方法"，在設定完後仍會回傳 `undefined`

```js
var str = "Hello";
console.log(typeof str); // "string"

var strObj = new String("Hello");
console.log(typeof strObj); // object

str.color = "red";
strObj.color = "red";

console.log(str.color); // undefined
console.log(strObj.color); // 'red'
```

## 物件與原型鍊 Object & Prototype Chain

JavaScript 是一門物件導向的程式語言，它的繼承方法是透過 "原型" `Prototype` 來操作

### 原型鍊 Prototype Chain

原型繼承的基本概念就是讓沒有該屬性的物件去存取其他物件的屬性，原型物件是物件的內部屬性，無法直接存取，通常直接標示為 `[[prototype]]`

#### Object.setPrototypeOf("繼承者", "原型")

透過 `Object.setPrototypeOf()` 可以指定物件之間的原型關係，第一個參數為繼承者物件，第二個為要繼承的原型物件

```js
var person = {};
var person2 = {
  name: "Jerry",
};

console.log(person.name); // undefined
console.log(person2.name); // 'Jerry'

Object.setPrototypeOf(person, person2);

console.log(person.name); // 'Jerry'
console.log(person2.name); // 'Jerry'
```

延續上方範例，同一物件無法指定兩個原型物件，如果繼承超過一個原型物件，只會繼承最後一個

```js
var age = {
  age: 18,
};
Object.setPrototypeOf(person, age);
console.log(person.age); // 18
console.log(person.name); // undefined
```

這時候就需要 "原型鍊" 的概念了，延續上方範例，先讓 `person2` 去繼承 `age`，再讓 `person` 繼承 `person2`

```js
var person = {};

var person2 = {
  name: "Jerry",
};

var age = {
  age: 18,
};

Object.setPrototypeOf(person2, age);
Object.setPrototypeOf(person, person2);

console.log(person.age); // 18
console.log(person.name); // 'Jerry'
```

### 最頂層原型物件 Object.prototype

當我們嘗試存取不存在的物件屬性時，就會自動往該物件的 "原型物件" `[[prototype]]` 去尋找，順著原型鍊一直找，直到 "最頂層原型物件" `Object.prototype`

最頂層原型物件 `Object.prototype` 是所有物件的起源，我們以往呼叫卻未定義的 "方法" `Method` 就是透過 "原型鍊" 繼承自這個 "最頂層原型物件" `Object.prototype` 的方法

```js
Object.prototype.hasOwnPrototype();
Object.prototype.toString();
Object.prototype.valueOf();
```

### 建構式與原型

因為函式也是物件的一種，所以也可以透過 `prototype` 來擴充每個透過這個函式所建構出來的物件

```js
var Person = function () {};

Person.prototype.sayHello = function () {
  return "Hi !";
};

var p1 = Person();
var p2 = new Person();

console.log(p1); // undefined
console.log(p2); // Person {sayHello: f}
console.log(p2.sayHello()); // "Hi !"
console.log(Person.sayHello()); // Error
```

這裡一一解析以上的結果以釐清觀念

- `p1`: 直接呼叫 `Person` 這個函式，所以就會是這個函式回傳的結果，因為沒有設定 return ，自然也就是預設的 undefined
- `p2`: 透過 `Person` 這個函式物件所建構出來的 "物件"
- `p2.sayHello()`: 函式被建立時，都會有原型物件，當我們透過這個函式建構一個新物件時，這個函式的原型物件就會被當作新物件的 "原型"
- `Person.sayHello()`: 因為 `Person` 本身是一個函式，所以並不會存取物件屬性

如果新建構出來的物件實體和原型具有相同的 "屬性" 和 "方法" 時，會優先存取自己的

```js
var Person = function () {
  this.sayHello = function () {
    return "Hello !";
  };
};

Person.prototype.sayHello = function () {
  return "Hi !";
};

var p = new Person();
console.log(p); // Person {sayHello: function (){return "Hello !"}}
console.log(p.sayHello); // "Hello !"
```

### 繼承判斷

透過 `hasOwnProperty()` 可以判斷該屬性是物件本身所有還是繼承而來的

```js
var Person = function () {
  this.name = "Jerry";
};

Person.prototype.age = 18;

var p = new Person();
console.log(p.name); // "Jerry"
console.log(p.age); // 18
console.log(p.hasOwnProperty("name")); // true
console.log(p.hasOwnProperty("age")); // false
```

### 物件的 `__proto__` 屬性

從上方的 [建構式與原型](#建構式與原型) 中可以知道，當透過函式建構出一個新的物件時，這個新物件的原型會自動指向函式的原型物件 `prototype`

在 JavaScript 中，所有物件都有 `__proto__` 這個特殊屬性，這個屬性會指向這個物件的原型物件 ; 在 ES5 中，也可以透過 `Object.getPropertyOf()` 這個方法來取得物件的原型物件

```js
var Person = function (name) {
  this.name = name;
};

var p = new Person("Jerry");

Person.prototype.sayHello = function () {
  return "Hi! " + this.name;
};

console.log(p.sayHello());

console.log(Object.getPrototypeOf(p) === Person.prototype); // true
console.log(Object.getPrototypeOf(p) === Function.prototype); // false
console.log(Object.getPrototypeOf(Person) === Function.prototype); // true

console.log(p.__proto__ === Person.prototype); // true
console.log(p.__proto__ === Function.prototype); // false
console.log(Person.__proto__ === Function.prototype); // true
```

要注意的是，當一個函是被建立的時候，會產生一個 `prototype` 屬性，但 `prototype` "並非" 這個函式的原型物件，而是透過這個函式所建構的新物件才會將原型指向函式的 `prototype` 屬性

```js
var Person = function (sayHello) {
  this.sayHello = sayHello;
};

var p = new Person("Hello");

console.log(p.sayHello); // Hello
console.log(Person.sayHello); // undefined
```

### 物件與原型繼承

前面都是透過建構式來介紹，但直接透過物件來繼承是比較常見的做法

- 第一種是上方所提到的 Object.setPropertyOf()

  ```js
  Object.setPropertyOf(p, Person);
  ```

- 第二種是透過 `Object.create()` 來建立一個新物件

  ```js
  var Person = {
    name: "Jerry",
    sayHello: function () {
      return "Hi! " + this.name;
    },
  };

  var p = Object.create(Person);

  console.log(p.sayHello()); // "Hi! Jerry"
  ```

而 `Object.create()` 背後的原理是將物件作為參數帶入，先建立一個新函式，再將這個函式的原型指向這個物件，最後回傳一個 `new` 的新物件

```js
Object.create = function (proto) {
  function F() {}
  F.prototype = proto;
  return new F();
};
```

### **proto** & prototype

總結一下` __proto__` & `prototype` 的關係，JavaScript 的內建物件 ` Array``、Function ` 等，這些物件的 `prototype` 屬性本身也是一個物件，而這些屬性實際上都是繼承自最頂層物件 `Object.prototype`

```js
console.log(Object.getPrototypeOf(Function.prototype) === Object.prototype); // true
console.log(Function.prototype.__proto__ === Object.prototype); // true
```

由此可知下層物件的 `__proto__` 屬性都會指向上層物件的 `prototype` 屬性，這就是 JavaScript 的繼承關係

## 資料參考

[物件與原型鏈](https://ithelp.ithome.com.tw/articles/10194154)
[【克服 JS 】第四章（上）物件與函式](https://chupainotebook.blogspot.com/2019/01/js.html)
[Object.defineProperty()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)
