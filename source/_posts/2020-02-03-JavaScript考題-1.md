---
title: JavaScript 考題 1
date: 2020-02-03
tags: 
  - w3HexSchool
  - js
---

<img src="https://i.imgur.com/0F9EHda.jpg" width="1024" alt="Meow Meow Picture" />

## 目錄

  - [作用域、提升](#hoisting)
  - [表達式、陳述式](#expression-declarative)
  - [變數](#variable)
  - [ASI](#asi)

<!-- more -->

<br>

<h2 id="hoisting">作用域、提升</h2>

`Hoisting`

  1.  函式陳述式優先
  2.  var 變數

<br>

### 測驗：請問結果為？

```js
var a = 0;
function a() {};
a();
```

> 1. 0
> 2. undefined
> 3. error

<br>

解答：

```js
// 創造
function a() {};
var a;

// 執行
a = 0;  // 這步驟就會把 函式 給覆蓋掉
a();    // 這裡其實是 a = 0
```

<br>

### 測驗：請問結果為？

```js
function b() {
  console.log(a);
  function a() {};
  var a = 3;
}
b();
```

> 1. f() {}
> 2. undefined
> 3. 3

<br>

### 測驗：請問結果為？

```js
function b() {
  console.log(a);
  var a = 3;
  function a() {};
}
b();
```

> 1. f() {}
> 2. undefined
> 3. 3

<br>

### 測驗：請問結果為？

```js
function b() {
  var a = 3;
  console.log(a);
  function a() {};
}
b();
```

> 1. f() {}
> 2. undefined
> 3. 3

<br>

### 測驗：請問結果為？

```js
var b;

function fn() {
  var a = b = 3;
};

fn();

console.log(typeof(a), typeof(b));
```

> 1. undefined, undefined
> 2. undefined, number
> 3. number, number

<br>

解答：

`typeof`

會將 `is not defined` 回傳 `undefined`

```js
var b;

function fn() {
  var a = b = 3;
  console.log(a); // 3
};

fn();
console.log(a); // a is not defined
console.log(typeof(a), typeof(b));
```

<br>

### 測驗：請問結果為？

```js
function fn() {
  a = 2;
  var a = 1;
};

var a = 3;
fn();
console.log(a);
```

> 1. 1
> 2. 2
> 3. 3

<br>

### 測驗：請問結果為？

```js
function fn() {
  a = 2;
};

var a = 3;
fn();
console.log(a);
```

> 1. 1
> 2. 2
> 3. 3

<br>

<h2 id="expression-declarative">表達式、陳述式</h2>

`函式陳述式`

```js
function foo() {}
```

<br>

`函式表達式`

```js
var fn = function() {}
```

<br>

### 測驗：請問結果為？

```js
var x = (y = 10);
console.log(x);
```

> 1. 10
> 2. undefined

<br>

解答：

```js
(y = 10)  // 這是一個表達式，括號優先執行所以回傳為 10，這個值就賦予到 x。
```

<br>

### 額外問題：y 是什麼？

    1. 被宣告的變數
    2. 未宣告的全域變數
    3. () 內的區域變數

<br>

解答：

    y 沒有用 var 宣告，所以不是被宣告變數。
    () 不是一個函式，所以不是區域變數。

<br>

### 測驗：請問結果為？

```js
var a = {
  a: 10
};

var b = a.a = a.a * a.a;

console.log(b)
```

> 1. 10
> 2. 100

<br>

解答：

```js
// 不要使用連續賦值
// 這是一個表達式，會回傳值並賦值於 var b。
a.a = a.a * a.a
```

<br>

### 測驗：請問結果為？

```js
var a = {
  a: 10
};

Object.defineProperty(a, 'a', {
  writable: false
});

var b = a.a = a.a * a.a;

console.log(b)
```

> 1. 10
> 2. 100

<br>

解答：

```js
// 會讓 a 物件 裡面的 'a' 屬性的值不能再被賦值
Object.defineProperty(a, 'a', {
  writable: false
});
```

<br>

### 測驗：請問結果為？

```js
var newList = [1, 2, 3].push(4);
console.log(newList.push(5));
```

> 1. [1, 2, 3, 4, 5]
> 2. [1, 2, 3, 5]
> 3. [1, 2, 3, 4]
> 4. Error

<br>

解答：

```js
// 這是表達式，回傳值為 4
[1, 2, 3].push(4)
```

```js
// 正確寫法
var newList = [1, 2, 3];
newList.push(4);
console.log(newList.push(5));   // 5
console.log(newList);           // [1, 2, 3, 4, 5]
```

<br>

<h2 id="variable">變數</h2>

- 未宣告的變數會成為全域變數
- **屬性** 可以被刪除，**變數** 則無法

```js
var a = 1;
b = 2;    // window 下的屬性
delete a; // false
delete b; // true
```

<br>

### 函式內的變數一定要宣告，才可以避免汙染到全域！

```js
// 錯誤寫法
function fn () {
  b = 3;
}
fn();
console.log(b);   // 3
```

```js
// 正確寫法
// 有宣告 var 就只會在函式作用域中
function fn () {
  var b = 3;
}
fn();
console.log(b);   // Uncaught ReferenceError: b is not defined
```

<br>

### 測驗：請問結果為？

```js
(function() {
  var a = b = 3;
}());
console.log(typeof a);
console.log(typeof b);
```

> 1. undefined, number
> 2. number, number

<br>

解答：

```js
// 因為函式內有宣告 var a 所以外層讀不到值
// 又因為 typeof 保護機制，所以會顯示為 undefined。
console.log(typeof a);    // undefined
```

<br>

#### 額外補充

將 `typeof` 刪除後重新執行，因為程式是逐行執行，

所以在 `console.log(a);` 會報錯就停住了。

`console.log(b);` 就不會執行！

```js
(function() {
  var a = b = 3;
}());
console.log(a);   // Uncaught ReferenceError: a is not defined
console.log(b);
```

<br>

### 測驗：請問結果為？

```js
var a;
var b;
(function() {
  a = b = 3;
  var b;
}());
console.log(typeof a);
console.log(typeof b);
```

> 1. undefined, number
> 2. number, number
> 3. number, undefined

<br>

<h2 id="asi">ASI</h2>

ASI：自動插入分號

ex: `continue`, `return`, `break`, `throw` 後方自動補上分號

[不會發生 ASI](https://www.udemy.com/course/javascript-adv/learn/lecture/15793656#questions)

<br>

### 測驗：請問結果為？

```js
var a = 1
  ,b = 2
delete a;
delete b;
console.log(typeof a,typeof b);
```

> 1. number, undefined
> 2. number, number
> 3. undefined, undefined

<br>

### 測驗：請問結果為？

```js
var a = 10
(function() {
  console.log(a)
}())
```

> 1. 10
> 2. 10 is not a function

<br>

解答：

```js
// 括號 前面不會自動加入分號
var a = 10(function() {
  console.log(a)
}())
```

```js
// 正確寫法
var a = 10;
(function() {
  console.log(a)  // 10
}())
```

```js
// 正確寫法
// 或者在 IIFE 前面都加上 ;
var a = 10

;(function() {
  console.log(a)  // 10
}())
```

<br>

### 測驗：請問結果為？

```js
function nums(a, b) {
  if
  (a > b)
  console.log('a is bigger')
  else
  console.log('b is bigger')
  return
  a + b
}
console.log(nums(4, 2))
console.log(nums(1, 2))
```

> 1. a is bigger, 6 and b is bigger, 3
> 2. a is bigger, undefined and b is bigger, undefined
> 3. undefined and undefined
> 4. SyntaxError

<br>

圖片來源： <a href="https://pngtree.com/free-backgrounds">free background photos from pngtree.com</a>