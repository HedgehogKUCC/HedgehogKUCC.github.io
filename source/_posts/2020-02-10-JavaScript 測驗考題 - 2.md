---
title: JavaScript 測驗考題 - 2
date: 2020-02-10
tags: 
  - w3HexSchool
  - js
---

<img src="https://i.imgur.com/zPsnTnb.jpg" width="1024" alt="JavaScript 核心測驗考題" />

<!-- more -->

# JavaScript 核心測驗考題

## 型別

- 六個原始型別以及一個物件型別 （沒有其它了）

[JavaScript 的資料型別與資料結構](https://paper.dropbox.com/ep/redirect/external-link?url=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-TW%2Fdocs%2FWeb%2FJavaScript%2FData_structures&hmac=V7dbov%2Brzphz7121EHGlJztAExWR%2FRIVHhGP7GA76m8%3D)

- 透過 typeof 查看型別
- 物件型別可以擴增屬性，原始型別則否。
- 型別可以動態轉換

    原始型別中的 String, Number, BigInt, Boolean, Symbol 
    皆具有包裹物件（ 所有方法都可以在裡面查到 ）
    [Primitive_wrapper_objects_in_JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/Primitive#Primitive_wrapper_objects_in_JavaScript)

<br>

### 測驗：請問結果為？

```js
console.log(typeof []);
```

> 1. object
> 2. array

<br>

### 測驗：請問結果為？

```js
function a() {};
console.log(typeof a);
```

> 1. object
> 2. function

<br>

### 測驗：請問結果為？

```js
function a() {
  this.a = 1;
}
a.a = 2;

console.log(a.a)
```

> 1. 1
> 2. 2

<br>

### 測驗：請問結果為？

```js
function a() {
  this.a.a = 1;
}
a.a = 2;
a();
console.log(a.a);
```

> 1. 1
> 2. 2

<br>

### 測驗：請問結果為？

```js
console.log(typeof typeof 1)
```

> 1. string
> 2. number
> 3. object

<br>

### 測驗：請問結果為？

```js
var a = [1, 2, 3];
a[19] = 20;
console.log(a.length);
```

> 1. 19
> 2. 20
> 3. error

<br>

### 測驗：請問結果為？

```js
var a = 'a';
console.log(Number(a = 10));
```

> 1. NaN
> 2. 10
> 3. error

<br>

## 包裹物件建構函式

- 使用 new + 包裹物件建立的會是物件型別，而不是原始型別。
- 盡可能不使用此方式建立

<br>

### 測驗：請問結果為？

```js
var a = 10;
var b = new Number(10);
console.log(a == b);
```

> 1. true
> 2. false

<br>

### 測驗：請問結果為？

```js
var a = 10;
var b = new Number(10);
console.log(a === b);
```

> 1. true
> 2. false

<br>

### 測驗：請問結果為？

```js
var a = new Number(10);
var b = new Number(10);
console.log(a == b);
```

> 1. true
> 2. false

<br>

### 測驗：請問結果為？

```js
var a = 10;
a.a = 5;
console.log(a.a)
```

> 1. 10
> 2. 5
> 3. undefined

<br>

### 測驗：請問結果為？

```js
var a = new Number(10);
a.a = 5;
console.log(a.a);
```

> 1. 10
> 2. 5
> 3. error

<br>

## 運算子

[運算式與運算子](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Expressions_and_Operators)

- [邏輯](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Expressions_and_Operators#%E9%82%8F%E8%BC%AF%E9%81%8B%E7%AE%97%E5%AD%90)
- [條件運算子](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Conditional_Operator#%E5%8F%82%E6%95%B0)
- [增加](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Increment)

<br>

`1 + 1`

`1` 為 運算元

`+` 為 運算子

<br>

### 測驗：請問結果為？

```js
var a = 1;
var b = delete a;
console.log(b);
```

> 1. error
> 2. true
> 3. false

<br>

解答：

`delete` 為 運算子

<br>

### 測驗：請問結果為？

```js
var a = 1;
a++;
var b = a++;
console.log(b);
```

> 1. 1
> 2. 2
> 3. 3

<br>

### 測驗：請問結果為？

```js
if (new Boolean(0)) {
  console.log('true')
} else {
  console.log('false');
}
```

> 1. true
> 2. false

<br>

解答：

```js
Boolean(0)  // false
new Boolean(0)  // undefined 為 true
```

<br>

### 測驗：請問結果為？

```js
function fn(n) {
  return n > 1 ? n * fn(n - 1) : n;
}
console.log(fn(3));
```

> 1. 3
> 2. 6
> 3. 7

<br>

### 測驗：請問結果為？

```js
    var a = 10;
    console.log(++a * a);
    a = 10;
    console.log(--a * a);
```

> 1. 110, 90
> 2. 121, 81
> 3. 100, 100

<br>

## 優先性、相依性

[運算子優先序](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

`運算子優先序`（Operator precedence）：

決定了運算子彼此之間被語法解析的方式，

優先序較高的運算子會成為優先序較低運算子的運算元（operands）。

- 當優先序 **相同** 時，使用相依性決定運算方向。

<br>

### 測驗：請問結果為？

```js
console.log(typeof 'a' + 'a')
```
> 1. string
> 2. stringa

<br>

### 測驗：請問結果為？

```js
var a = 10;
console.log(a++ * a++);
```

> 1. 100
> 2. 110
> 3. 121

<br>

### 測驗：承上題 a 的值為？

```js
var a = 10;
console.log(a++ * a++);
console.log(a);
```

> 1. 10
> 2. 11
> 3. 12

<br>

## 判斷式

### 測驗：請問結果為？

```js
var a = 10;
var b = 0;
var c = b++ || a;
console.log(c);
```

> 1. 10
> 2. 0
> 3. 1

參考 [Logical operators](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Logical_Operators)

<br>

### 測驗：請問結果為？

```js
console.log(10 == 10n);
console.log(10 === 10n);
```

> 1. true true
> 2. true false
> 3. false false

<br>

### 測驗：請問結果為？

```js
3 < 2 == 0
```

> 1. true
> 2. false
> 3. 0
> 4. 1

參考 [運算子優先序](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

<br>

### 額外補充

```js
// true
1 < 2 < 3

// false
3 > 2 > 1
```

<br>

## 數字型別運算的技巧

- 只有 **加法** 不太一樣，因為他有 "字串相接" 的功能，所以遇到字串、物件型別會有不同結果
- 減乘除都可以使用 Number 來轉型 ( 看是否能正確轉型 )

<br>

### 測驗：請問結果為？

```js
var a = '1';
console.log(typeof a)
a = +a;
console.log(typeof a)
a = a + '';
console.log(typeof a)
```

> 1. string string string
> 2. string number number
> 3. string number string

<br>

### 測驗：請問結果為？

```js
console.log(100 === '10' * 10)
```

> 1. true
> 2. false

<br>

圖片來源： <a href="https://pngtree.com/free-backgrounds">free background photos from pngtree.com</a>