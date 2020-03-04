---
title: JavaScript 考題 3
date: 2020-02-17
tags: 
  - w3HexSchool
  - js
---

<img src="https://i.imgur.com/m0EStr7.jpg" width="1024" alt="Meow Meow Picture" />

## 物件

<!-- more -->

- 物件的基本運作
- 純值與物件的差異
- 物件的參考特性

<br>

### 原始碼是否正確呢？

```js
var person = {
  name: '小明',
  age: 32,
  1: '2',
  gender: 'male',
  interests: ['吃飯', '睡覺', '打動動'],
  greeting: function() {
    console.log("Hi! I'm " + this.name[0] + '.');
  },
  哈囉: function() {
    console.log('我是' + this.name);
  }
};
```

<br>

解答：

- 正確
- 屬性都是字串
- 屬性都是唯一的

```js
// 如何取得上述的所有屬性值

person.name
person.age
person.gender
person.interests
person.greeting()

person.1  // Uncaught SyntaxError: Unexpected number
person[1]
person['1']

person[哈囉]  // Uncaught ReferenceError: 哈囉 is not defined
person.哈囉
person.哈囉()
person['哈囉']()
```

<br>

## 純值與物件的差異

- 物件可以自由擴增 “屬性”，純值則無法。

<br>

### 範例程式碼

```js
function callName() {
  console.log('呼叫小明');
}
callName.ming = '小明';
```

<br>

### 測驗：請問結果為？

```js
var a = 1;
a.a = 2;
console.log(a.a);
```

> 1. 1
> 2. 2
> 3. undefined
> 4. error

<br>

### 測驗：請問結果為？

```js
var a = new Number(1);
a.a = 2;
console.log(a.a)
```

> 1. 1
> 2. 2
> 3. undefined

<br>

### 測驗：請問結果為？

```js
function fn() {
  return 1;
}
fn.return = 2;
console.log(fn.return);
```

> 1. 1
> 2. 2
> 3. undefined

<br>

### 測驗：請問結果為？

```js
var a = 1;
function fn() {
  a = fn.a;
}
fn.a = 2;
console.log(a);
```

> 1. 1
> 2. 2
> 3. undefined

<br>

### 範例程式碼：

```js
var family = {
  name: '小明家',
  members: {
    father: '老爸',
    mom: '老媽',
    ming: '小明'
  },
};
var member = family.members;
// 問題 重新賦予值是否影響
member = {
  ming: '大明'
};
// 問題 2 修改單一屬性是否有影響
member.ming = '杰倫';
var ming = family.members.ming;
```

<br>

### 測驗：請問結果為？

```js
var a = {};
var b = { b: 'c' };
var c = { c: 'b' };
a[b.b] = 123;
a[c.c] = 456;
console.log(a['b']);
```

> 1. 123
> 2. 456
> 3. undefined

<br>

### 補充

```js
var a = {
  '哈囉': 1,
}

var hello = '哈囉';

console.log(a[hello]);  // 1
```

<br>

### 測驗：請問結果為？

```js
var a = {};
var b = { key: 'b' };
var c = { key: 'c' };
a[b] = 123;
a[c] = 456;
console.log(a[b]);
```

> 1. 123
> 2. 456
> 3. undefined

    NOTE: 屬性都是 字串

<br>

解答：

```js
String(b) // [object, object]

console.log(a)  // { [object, object]: 456 }
```

<br>

### 測驗：請問結果為？

```js
var prop = '1';
var obj = {
  1: function() {
    console.log('1');
  },
  prop: function() {
    console.log('2');
  }
};
obj[prop]();
```

> 1. 1
> 2. 2
> 3. undefined
> 4. error

<br>

### 測驗：請問結果為？

```js
var a = {};
a.a = a;
console.log(a.a.a === a.a);
```

> 1. true
> 2. false
> 3. error

    NOTE: 重要觀念題！！！

<br>

### 測驗：請問結果為？

```js
var a = { x: 1 };
var b = a;
a.x = a = { x: 2 };

console.log(a);
console.log(b);
```

    NOTE: 重要觀念題！！！

<br>

### 測驗：請問結果為？

```js
var a = {};
var b = (a = {});
console.log(a === b);
```

> 1. true
> 2. false
> 3. error

    NOTE: 重要觀念題！！！

<br>

圖片來源： <a href="https://pngtree.com/free-backgrounds">free background photos from pngtree.com</a>