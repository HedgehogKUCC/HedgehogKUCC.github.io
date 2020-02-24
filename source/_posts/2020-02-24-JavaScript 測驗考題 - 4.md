---
title: JavaScript 測驗考題 - 4
date: 2020-02-24
tags: 
  - w3HexSchool
  - js
---

<img src="https://i.imgur.com/44eSpjJ.jpg" width="1024" alt="JavaScript 核心測驗考題" />

<!-- more -->

# JavaScript 核心測驗考題

## 函式

- 函式陳述式、具名函式、匿名函式、函式表達式
- 函式與參數
- 閉包 ( Closure )
- this

<br>

### 函式陳述式 & 具名函式

```js
function functionA() {
  console.log('函式陳述式、具名函式');
  console.log(functionA);
}
functionA();
```

<br>

### 函式表達式 & 匿名函式

```js
var functionB = function() {
  console.log('函式表達式 B');
  console.log(functionB); // 函式沒有名稱
}
functionB();
```

<br>

### 函式表達式 & 具名函式

```js
var functionC = function functionD(log) {
  console.log(log);
  console.log(functionC, functionD); // 函式名稱為 functionD
  console.dir(functionD); // 這裡可以呼叫到 FunctionD，但外層不行！
}
functionC('函式表達式 C'); // 函式參數
console.log(functionD); // Uncaught ReferenceError: functionD is not defined
```

```js
var num = 1;
var giveMeMoney = function giveMoreMoney(coin) {
  num += 1;
  console.log('執行 giveMeMoney : ', num, coin);
  return coin > 100 ? coin : giveMoreMoney(num * coin);
};

giveMeMoney(30);
```

```js
function CallSomeOne(fn) { 
  fn(); 
  console.log(fn);
};

CallSomeOne(function () { 
  console.log('執行函式'); 
});
```

<br>

### 立即函式 & 具名函式

```js
(function IIFE() {
  console.log('這是一段立即函式', IIFE);
}());
```

    NOTE: 立即函式通常都是匿名函式，具名函式才可以在呼叫自己。

<br>

### 補充說明

在立即具名函式中在執行自己 `IIFE()` 會造成無限迴圈產生 Error

`Uncaught RangeError: Maximum call stack size exceeded`

```js
(function IIFE() {
  console.log('這是一段立即函式', IIFE());
}());
```

<br>

```js
(function () {
   
})()
(function () {
   
})()

// Uncaught TypeError: (intermediate value)(...) is not a function
// 一定要在立即函式前面或後面加上 ;
```

```js
// 正確寫法
(function () {
   
})();
(function () {
   
})()
```

```js
// 正確寫法
(function () {
   
})()
;(function () {
   
})()
```

```js
(function (global) {
   global.person = '小明'
})(window);
(function () {
    console.log(person)
})()
```

<br>

### 測驗，請問結果為？

```js
var name = '全域'

var obj = {
    name: '區域',
    callName: function () {
        console.log(this.name)
    }
};

(function () {
    var a = obj.callName
    a()
})()
```

> 1. 全域
> 2. 區域
> 3. error

<br>

### 測驗，請問結果為？

```js
var name = '全域'

var obj = {
    name: '區域',
    callName: function () {
        console.log(this.name)
    }
}

(function () {
    var a = obj.callName
    a()
})()
```

> 1. 全域
> 2. 區域
> 3. error

<br>

### 測驗，請問結果為？

```js
var name = '全域'

var obj = {
    name: '區域',
    callName: function () {
        console.log(this.name)
    }
};

(function () {
    obj.callName()
})()
```

> 1. 全域
> 2. 區域
> 3. error

<br>

## 閉包 Closure

> JS 的記憶體釋放特性：當變數無法再被關聯時就會釋放記憶體

Example:

```js
function randomString(length) {
  var result = '';
  var characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
  var charactersLength = characters.length;
  for (var i = 0; i < length; i++) {
    result += characters.charAt(Math.floor(Math.random() * charactersLength));
  }
  return result;
}

// var demoData = [];

function getData() {
  var demoData = [];
  for (let i = 0; i < 1000; i++) {
    demoData.push(randomString(1000))
  }
  // console.log(demoData);  // 注意： Chrome console 行為也是需要記憶體
}

// 當變數無法再被取用時，記憶體就會釋放。
getData();
```

<br>

### 因為變數可以再次被存取，所以可以重複不斷取得相同的變數。

```js
function storeMoney() {
  var money = 1000;
  return function (price) {
    money = money + price;
    return money;
  }
}
```

```js
var myMoney = storeMoney()(100);  // 存取內部函式的變數
console.log(myMoney); // 1100
```

```js
var myMoney = storeMoney();  // 存取內部函式的變數
console.log(myMoney(100));  // 1100
console.log(myMoney(100));  // 1200
console.log(myMoney(100));  // 1300
```

```js
// 賦予在不同變數上運算可以獨立計算

// 小明錢包
var MingMoney = storeMoney();
console.log(MingMoney(100));
console.log(MingMoney(100));
console.log(MingMoney(100));

// 傑倫錢包
var JayMoney = storeMoney();
console.log(JayMoney(500));
console.log(JayMoney(100));
console.log(JayMoney(500));
```

    NOTE: 閉包，透過 A函式內 B函式取用 A函式內變數，避免 A函式內變數的記憶體被釋放！讓 A函式內的變數可以被重複存取。

<br>

### 測驗：請問結果為？

```js
var name = '小明';
function easyCard(base = 100) {
  var money = base;
  return function(update = 10) {
    money = money + update;
    return money;
  }
}

var MingEasyCard = easyCard(100);
MingEasyCard();
MingEasyCard();
console.log(MingEasyCard);
```

> 1. ƒ
> 2. 110
> 3. 120
> 4. 130

<br>

### 測驗：請問結果為？

```js
var name = '小明';
function easyCard(base = 100) {
  var money = base;
  return function(update = 10) {
    money = money + update;
    return money;
  }
}

var MingEasyCard = easyCard(100);
MingEasyCard();
MingEasyCard();
console.log(MingEasyCard());
```

> 1. 110
> 2. 120
> 3. 130

<br>

## This

### 影響 This 的方法

- 簡易呼叫 → 盡可能不要使用 this
- 作為物件方法
- bind, apply, call
- new
- DOM 事件處理器
- 箭頭函式 (函式)

    非常重要： This 的指向與函式在哪建立無關，只跟調用方式有關。

<br>

### 簡易呼叫

```js
var myName = '全域';

function callName() {
  console.log(this);
  console.log(this.myName);
}

callName();
```

[this 為什麼指向 window](https://wcc723.github.io/javascript/2019/03/21/this-why-window/)

<br>

### 作為物件方法

```js
var myName = '全域';

function callName() {
  console.log(this);
  console.log(this.myName);
}

var family = {
  myName: '小明家',
  callName: callName
};

family.callName();
```

<br>

```js
// this 指向 window

var family = {
  myName: '小明家',
  callName: function () {
    var vm = this;
    setTimeout(function (){
      console.log(this);
    }, 0);
  }
}
family.callName();
```

```js
// vm 指向 family

var family = {
  myName: '小明家',
  callName: function () {
    var vm = this;
    setTimeout(function (){
      console.log(vm);
    }, 0);
  }
}
family.callName();
```

<br>

### call & apply

```js
// call 第一個參數作為 this 使用

var family = {
  myName: '小明家',
}

function fn(para1, para2) {
  console.log(this, para1, para2);
}

fn.call(family);  // { myName: "小明家" } undefined undefined
fn.call(family, 1, 2);  // { myName: "小明家" } 1 2
fn.apply(family, [1, 2]); // { myName: "小明家" } 1 2
```

    NOTE: call 和 apply 呼叫方式很像，只是 apply 後面參數要用 array。

<br>

### bind

```js
var family = {
  myName: '小明家',
}

function fn(para1, para2) {
  console.log(this, para1, para2);
}

var fn2 = fn.bind(family);

fn2();  // {myName: "小明家"} undefined undefined
```

```js
// bind 可以直接帶參數

var family = {
  myName: '小明家',
}

function fn(para1, para2) {
  console.log(this, para1, para2);
}

var fn2 = fn.bind(family, '10');

fn2();  // {myName: "小明家"} '10' undefined
```

```js
// 但是在呼叫加入的參數，不會取代原本第一個參數，而是變成第二個參數！

var family = {
  myName: '小明家',
}

function fn(para1, para2) {
  console.log(this, para1, para2);
}

var fn2 = fn.bind(family, '10');

fn2(1); // {myName: "小明家"} '10' 1
```

    NOTE: 
    由於使用 bind 賦予到變數後在執行跟簡易呼叫一樣
    所以如果不知道 bind 了什麼
    就會造成與預期結果不同的狀況！！！

<br>

### 測驗結果為何？

```js
var a = 1;
function fn() {
  this.a = fn.a;
}
fn.a = 2;
fn();
console.log(a);
```

> 1. 1
> 2. 2
> 3. undefined

<br>

### 測驗：請問結果為？ (系列題)

```js
var value = 1;
var foo = {
  value: 2,
  bar: function() {
    return this.value;
  }
};

console.log(foo.bar());
```

> 1. 1
> 2. 2

<br>

### 測驗：請問結果為？ (系列題 1)

```js
console.log((foo.bar = foo.bar)());
```
> 1. 1
> 2. 2

<br>

### 測驗：請問結果為？ (系列題 2)

```js
console.log((false || foo.bar)());
```

> 1. 1
> 2. 2

<br>

### 測驗：請問結果為？

```js
var a = {
  value: 1,
  fn: function() {
    return this.value;
  }
}

var b = a;
b.value = 2;
console.log(a.fn());
```

> 1. 1
> 2. 2

<br>

### 測驗：請問結果為？

```js
var a = 1;
function fn() {
  return this.a;
}
fn.a = 2;
fn.fn = fn;
console.log(fn.fn());
```

> 1. 1
> 2. 2
> 3. undefined
> 4. error

<br>

### 測驗：請問結果為？

```js
var value = 1;
var a = {
  value: 2,
  fn: function () {
    return this.value;
  }
}
a.fn.call(undefined);
```

> 1. 1
> 2. 2
> 3. undefined

<br>

解答：

1. 看起來是物件方法調用，但因為是用 `call` 來取代 `this`，所以這個 `this` 是以 `call` 為主，而不是物件下的方法作為調用！
2. 當 `call` 第一個參數帶入的是 `undefined`，會自動替換成 `window`。

<br>

### 測驗：請問結果為？ (系列題 1)

```js
// Closure

var clfn = function() {
  var obj = {
    fn: function() {
      this.value++;
      return this;
    },
    value: 1
  };
  return obj;
}
var b = clfn();
console.log(b.fn() === b.fn());
```

> 1. true
> 2. false

    NOTE: 非常重要的觀念題

<br>

### 測驗：請問結果為？ (系列題 2)

```js
var clfn = function() {
  var obj = {
    fn: function() {
      this.value++;
      return this;
    },
    value: 1
  };
  return obj;
}
var b = clfn();
var c = clfn();
console.log(b.fn() === c.fn());
```

> 1. true
> 2. false

    NOTE: 非常重要的觀念題

<br>

圖片來源： <a href="https://pngtree.com/free-backgrounds">free background photos from pngtree.com</a>