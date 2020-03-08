---
title: JavaScript 考題 5
date: 2020-03-02
tags: 
  - w3HexSchool
  - js
---

<img src="https://i.imgur.com/0F9EHda.jpg" width="1024" alt="Meow Meow Picture" />

## 原型

<!-- more -->

    實體 __proto__   =>   陣列 (原型)   =>   物件 (原型)

- 哪裡可以看到原型
    - 陣列
    - 包裹物件、建構函式
- 自定義原型
- 原型鍊完整結構
- defineProperty

<br>

### 從陣列看原型

```js
var a = [1, 2, 3];
var b = [4, 5, 6];
a.__proto__.getLast = function () {
  return this[this.length -1];
}
console.log(a, b);
console.log(a.getLast(), b.getLast());
```

<br>

### 補充說明

`__proto__`： 實體後的路徑 ( 方便驗證原型使用 )

`prototype`： 建構函式下的方法 ( 實際上會使用這個來建立原型 )

`function` 會自帶 `this` 、 `arguments`

`arguments`： 類陣列，沒有陣列的方法 ( EX: forEach )。且在箭頭函式內不能使用。

<br>

### 陣列與類陣列

```js
function callArg(a) {
  console.log(a, arguments);
  arguments.forEach(function() {});
}

callArg();  // Uncaught TypeError: arguments.forEach is not a function
```

<br>

### 包裹物件與建構函式

```js
var b = new String('bcde');
console.log(b);
console.dir(String);
String.prototype.lastText = function() {
  return this[this.length - 1];
}
console.log(b.lastText());
```

<br>

### 原型範例

```js
function Dog(name, color) {
    this.name = name;
    this.color = color;
}

var bibi = new Dog('bibi', 'white');

Dog.prototype.bark = function () {
    console.log(this.name + ' 汪汪');
}

bibi.bark();

bibi.__proto__ === Dog.prototype;   // true

bibi.__proto__.constructor === Dog;   // true

bibi.__proto__.__proto__ === Object.prototype;    // true

bibi.__proto__.__proto__.__proto__ === null;    // true
```

<br>

### 自定義原型

```js
// 建構函式 動物
function Animal(family) {
  this.kingdom = '動物界';
  this.family = family || '人科';
}
Animal.prototype.move = function() {
  console.log(this.name + ' 移動');
}

// 建構函式 狗
function Dog(name, color, size) {
  Animal.call(this, '犬科')
  this.name = name;
  this.color = color || '白色';
  this.size = size || '小';
}

// 透過 Object.create 讓 Dog 繼承在 Animal 下
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.bark = function () {
  console.log(this.name + ' 吠叫');
}

var Bibi = new Dog('比比', '棕色', '小');
console.log(Bibi);
Bibi.bark();
Bibi.move();

// 建構函式 貓
function Cat(name, color, size) {
  Animal.call(this, '貓科');
  this.name = name;
  this.color = color || '白色';
  this.size = size || '小';
}

// 透過 Object.create 讓 Cat 繼承在 Animal 下
Cat.prototype = Object.create(Animal.prototype);
Cat.prototype.constructor = Cat;

Cat.prototype.meow = function () {
  console.log(this.name + ' 喵喵叫');
}

var Kity = new Cat('凱蒂');
Kity.meow();
Kity.move();
Kity.bark();
```

<br>

### 測驗：請問結果為？

```js
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}
const member = new Person('Lydia', 'Hallie');
Person.getFullName = function() {
  return `${this.firstName} ${this.lastName}`;
};
console.log(Person.getFullName());
console.log(member.getFullName());
```

> 1. Lydia Hallie
> 2. error

<br>

### 測驗：請問結果為？

```js
function Car() {
  this.make = 'M';
}
const myCar = new Car();
console.log(myCar.make);
```

> 1. "M"
> 2. undefined
> 3. ReferenceError
> 4. TypeError

<br>

### 測驗：請問結果為？

```js
function Car() {
  this.make = 'M';
  return { make: 'S' };
}
const myCar = new Car();
console.log(myCar.make);
```

> 1. `"M"`
> 2. `"S"`
> 3. `ReferenceError`
> 4. `TypeError`

    NOTE: 千萬不要在建構函式內 return ！！！

<br>

### defineProperty - 屬性調整

    針對物件增加額外的屬性
    預設 writable、enumerable、configurable 為 true

```js
var person = {
  a: 1,
  b: 2,
  c: 3
}

Object.defineProperty(person, 'a', {
  value: 3,
  writable: false,  // 可否被寫入
  enumerable: false, // 列舉 ( 使用 forin 時，能否取得物件裡面的值 )
  configurable: false // 可否刪除
});
```

<br>

### Getter 與 Setter

```js
var wallet = {
  total: 100
};
Object.defineProperty(wallet, 'save', {
  configurable: true,
  enumerable: true,
  set: function(price) {
    this.total = this.total + price / 2;
  },
  get: function() {
    return this.total / 2;
  }
});

wallet.save = 300;
console.log(wallet);
console.log(Object.getOwnPropertyDescriptor(wallet, 'save'));
```

<br>

### 物件屬性擴增方法

```js
var person = {
  a: 1,
  b: 2,
  c: {}
}
Object.freeze(person);
console.log('是否被凍結', Object.isFrozen(person)); // true

// writable: false 、 enumerable: true 、 configurable: false
console.log('person a 的屬性特徵', Object.getOwnPropertyDescriptor(person, 'a'));

// 調整屬性
person.a = 'a';

// 新增屬性
person.d = 'd';

// 巢狀屬性調整
// freeze 的是淺層，所以在深層是可以被賦予屬性和值！
person.c.a = 'ca';

// 刪除
delete person.b;  // false
console.log('person 物件', person);

// 弱點
// 給予新的物件就被改變了
// 因為 freeze 的是參考路徑
person = {};
```

<br>

### 測驗：請問結果為？

```js
var box = { x: 10, y: 20 };
Object.freeze(box);
var shape = box;
shape.x = 100;
console.log(shape);
```

> 1. { x: 10, y: 20 }
> 2. { x: 100, y: 20 }
> 3. ReferenceError
> 4. TypeError

<br>

### 測驗：請問結果為？

```js
var box = { x: 10, y: 20 };
Object.freeze(box);
box = { x: 10, y: 20 };
var shape = box;
shape.x = 100;
console.log(shape);
```

> 1. { x: 10, y: 20 }
> 2. { x: 100, y: 20 }
> 3. ReferenceError
> 4. TypeError

<br>

### 測驗：請問結果為？ ( 非常重要 )

```js
function sayHi (name) {
  var name = 'Casper';
  this.name = name;
  return this.name;
}

sayHi.__proto__.hello = function (name) {
  return this(name);
}

console.log(sayHi.hello(name));

var name = 'Hello';
```

> 1. Casper
> 2. Hello
> 3. hello is not defined
> 4. undefined
> 5. sayHi

<br>

### 測驗：請問結果為？ ( 非常重要 )

```js
function sayHi () {
  var name = 'Casper';
  return name;
}

sayHi.name = 'Mark';

sayHi.__proto__.hello = function () {
  return this.name;
}

sayHi.hello();
```

> 1. Casper
> 2. Mark
> 3. sayHi
> 4. hello

<br>

### 測驗：請問結果為？

```js
var person = {
  name: '爸爸',
  age: 28
}

var family = Object.create(person);

console.log(family);
```

> 1. { name: '爸爸', age: 28 }
> 2. { }
> 3. undefined
> 4. 以上皆非

<br>

### 測驗：請問結果為？ ( 承上題 )

```js
var person = {
  name: '爸爸',
  age: 28
}

var family = Object.create(person);

console.log(family.name);
```

> 1. 爸爸
> 2. undefined
> 3. name is not define
> 4. { }

<br>

圖片來源： <a href="https://pngtree.com/free-backgrounds">free background photos from pngtree.com</a>