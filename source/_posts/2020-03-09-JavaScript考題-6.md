---
title: JavaScript 考題 6
date: 2020-03-09
tags: 
  - w3HexSchool
  - js
---

<img src="https://i.imgur.com/0F9EHda.jpg" width="1024" alt="Meow Meow Picture" />

## 目錄

  - [ES6](#es6)
  - [Arrow Function](#arrow)
  - [Template String](#tem-string)
  - [縮寫](#abbreviation)
  - [示範展開](#expand)
  - [其餘參數](#parameter)
  - [Promise](#promise)
  - [Debounce & Throttle](#dt)

<!-- more -->

<br>

<h2 id="es6">ES6</h2>

- Let, Const
- 箭頭函式
- 常見語法糖
    - 展開、其餘參數、縮寫函式、template string、Class
- Promise

<hr>

<br>

### Let, Const

基本概念

- let 可再次賦值、const 則無法
- 全域宣告時，不會在 window 上產生值
- 塊狀作用域
- 都無法重複宣告
- 暫時性死區

<br>

```js
var a;

// 作用域 function
// Hoisting
// 如果是全域，會在 window 產生屬性。
// 可以爽爽重複宣告
// =================================

// let、const
// 作用域 block
// TDZ
// 不會在 window 產生屬性
// 不能重複宣告
```

```js
for (var i = 0; i < 10; i++) {
  setTimeout(function () {
    console.log('這執行第' + i + '次');
  }, 0);
}
console.log(i);
```

```js
for (let i = 0; i < 10; i++) {
  setTimeout(function () {
    console.log('這執行第' + i + '次');
  }, 0);
}
console.log(i);
```

<br>

### Hoisting?

```js
let Ming = '小明';
{
  console.log(Ming); // 小明
}
```

```js
let Ming = '小明';
{
  // 創造
  // let Ming; // 暫時性死區 TDZ

  // 執行
  console.log(Ming); // Uncaught ReferenceError: Cannot access 'Ming' before initialization
  let Ming = '杰倫';
}
```

<br>

<h2 id="arrow">Arrow Function</h2>

- 寫起來簡單（很多縮寫方法）
- 縮寫時，自帶 return
- 沒有自己的 this
- 沒有 arguments 參數

<br>

### 縮寫以下範例

```js
var arr = [1, 2, 3, 4, 5];
var newArr = arr.filter(function(item) {
  return item > 3;
})
console.log(newArr);
```

```js
var arr = [1, 2, 3, 4, 5];

var newArr = arr.filter( item => item > 3);

console.log(newArr);
```

<br>

### this

```js
var name = '1';
var auntie = {
  name: '2',
  callSomeone() {
    setTimeout(function() {
      console.log(this.name)
    }, 100);
  }
}
auntie.callSomeone();
```

```js
var name = '1';
var auntie = {
  name: '2',
  callSomeone() {
    console.log(this)
    setTimeout(function() {}, 100);
  }
}
auntie.callSomeone();
```

```js
var name = '1';
var auntie = {
  name: '2',
  callSomeone() {
    var vm = this
    setTimeout(function() {
      console.log(vm.name)
    }, 100);
  }
}
auntie.callSomeone();
```

```js
var name = '1';
var auntie = {
  name: '2',
  callSomeone() {
    setTimeout( () => {
      console.log(this.name)
    }, 100);
  }
}
auntie.callSomeone();
```

<br>

### 測驗：請問結果為？

```js
var value = 1;
var obj = {
  value: 2,
  fn: () => {
    console.log(this.value);
  }
}
obj.fn();
```

> 1. 1
> 2. 2
> 3. undefined

<br>

### 測驗：請問結果為？

```js
const value = 1;
const obj = {
  value: 2,
  fn: () => {
    console.log(this.value);
  }
}
obj.fn();
```

> 1. 1
> 2. 2
> 3. undefined

<br>

<h2 id="tem-string">Template String</h2>

```js
const people = [
  {
    name: '小明',
    friends: 2
  },
  {
    name: '阿姨',
    friends: 999
  },
  {
    name: '杰倫',
    friends: 0
  }
];

// let originString = '我叫做 ' + people[0].name;
let originString = `我叫做 ${ people[0].name }`;

// let originUl = '<ul>\
//   <li>我叫做 ' + people[0].name + '</li>\
//   <li>我叫做 ' + people[1].name + '</li>\
//   <li>我叫做 ' + people[2].name + '</li>\
// </ul>';
let originUl = `<ul>
  ${ people.map( item => `<li>我叫做 ${ item.name }</li>`).join('')}
</ul>`;

console.log(originString)
console.log(originUl)
```

<br>

<h2 id="abbreviation">縮寫</h2>

```js
var obj = {
  a: 1,
  b: 2,
  // fn: function () { console.log('fn') }
  fn() {
    console.log('fn');
  }
};

var newObj = {
  // obj: obj
  obj
};

newObj.obj.fn();
```

<br>

<h2 id="expand">示範展開</h2>

```js
// arguments 為 類陣列，所以沒有陣列方法！
function updateEasyCard() {
  var arg = arguments
  var sum = arg.reduce(function (accumulator, currentValue) {
    return accumulator + currentValue;
  }, 0);
  console.log(sum);
}
updateEasyCard(10, 50, 100, 50, 5, 1, 1, 1, 500);

// Uncaught TypeError: arg.reduce is not a function
```

```js
// 透過展開 arguments 在給予新陣列，就可以使用陣列方法！
function updateEasyCard() {
  // var arg = arguments
  var arg = [...arguments]
  var sum = arg.reduce(function (accumulator, currentValue) {
    return accumulator + currentValue;
  }, 0);
  console.log(sum);
}
updateEasyCard(10, 50, 100, 50, 5, 1, 1, 1, 500);
```

<br>

<h2 id="parameter">其餘參數</h2>

```js
function updateEasyCard(...arg) {
  var sum = arg.reduce(function (accumulator, currentValue) {
    return accumulator + currentValue;
  }, 0);
  console.log(sum);
}
updateEasyCard(10, 50, 100, 50, 5, 1, 1, 1, 500);
```

```js
function updateEasyCard(a, ...arg) {
  console.log(a, arg)
  var sum = arg.reduce(function (accumulator, currentValue) {
    return accumulator + currentValue;
  }, 0);
  console.log(sum);
}
updateEasyCard(10, 50, 100, 50, 5, 1, 1, 1, 500);
```

<br>

<h2 id="promise">Promise</h2>

- resolve 接收成功
- reject 接收失敗

<br>

### Fetch API

可參考：[https://wcc723.github.io/javascript/2017/12/28/javascript-fetch/](https://wcc723.github.io/javascript/2017/12/28/javascript-fetch/)


<br>

```js
fetch('https://randomuser.me/api/')
    .then((res)=> {
      // 這裡會得到一個 ReadableStream 的物件
      // 可以透過 blob(), json(), text() 轉成可用的資訊
      return res.json();
    })
    .then(jsonData => {
      console.log(jsonData);
    });
```

<br>

### 測驗：

```js
const data = [];

fetch('https://randomuser.me/api/')
    .then((res)=> {
      return res.json()
    })
    .then(jsonData => {
      data.push(jsonData.results[0]);
    });

console.log(data);
```

> 1. [{…}]
> 2. [ ]

<br>

```js
function fn () {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('成功')
    }, 1000)
  })
}

fn().then(res => {
  console.log(res)
})
```

```js
function fn () {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('成功')
    }, 1000)
  })
}

function fn2() {
  return new Promise(function(resolve) {
    setTimeout(function() {
      resolve(3);
    }, 0);
  });
}

fn().then( res => {
  console.log(res)
  return fn2()
}).then( res => {
  console.log(res)
})
```

```js
function fn () {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('成功')
    }, 1000)
  })
}

function fn2() {
  return new Promise(function(resolve) {
    setTimeout(function() {
      resolve(3);
    }, 0);
  });
}

function fn3() {
  return new Promise(function(resolve, reject) {
    setTimeout(function() {
      reject(new Error('失敗'))
    }, 0);
  });
}

fn().then( res => {
  console.log(res)
  return fn2()
}).then( res => {
  console.log(res)
  return fn3()
}).then( res => {})
.catch( err => console.log(err))
```

<br>

### 封裝自己的 Promise

```js
function ajaxGet(seed) {
  return new Promise((resolve, reject) => {
    fetch(`https://randomuser.me/api/?seed=${seed}`)
      .then(res => {
        return res.json();
      })
      .then(jsonData => {
        resolve(jsonData);
      }).catch((err) => {
        reject(new Error('連不上'));
      });
  })
}

ajaxGet('fea8be3e64777240').then((res)=> {
  console.log(res);
  return ajaxGet('12345678')
}).then((res) => {
  console.log(res);
});
```

<br>

### 多個

```js
Promise.all([ajaxGet('fea8be3e64777240'), ajaxGet('12345678')]).then((res) => {
  console.log(res);
});
```

<br>

### Async, Await

```js
async function fnAsync() {
  const a = await ajaxGet('fea8be3e64777240');
  const b = await ajaxGet('12345678');
  // Await 後可以做很多處理，程式碼邏輯會比較簡單
  const dataA = a.results[0];
  const dataB = b.results[0];
  return [
    dataA,
    dataB
  ]
}
fnAsync().then((res) => {
  console.log(res);
})
```

<br>

## 同學新增的問題

```js
for (var i = 0; i < 5; i++) {
  setTimeout(function() {
    console.log(i);
  }, 0);
}
```

> 1.輸出為何？  2.如何輸出 0 到 4

<br>

解答：

```js
for (let i = 0; i < 5; i++) {
  setTimeout(function() {
    console.log(i);
  }, 0);
}
```

```js
for (var i = 0; i < 5; i++) {
  ((i) => {
    setTimeout(function() {
      console.log(i);
    }, 0);
  })(i)
}
```

<br>

### 測驗，請問結果為？

```js
var a = new Object();

if (a) console.log('running');
else console.log('running else');
```

> 1. running
> 2. running else

<br>

### 測驗，請問結果為？

```js
var a = '小名';

function fn(){
  console.log(a);
  var a = '小秀';
  console.log(a);
}

fn();
```

> 1. undefined, 小秀
> 2. 小名, 小秀

<br>

<h2 id="dt">Debounce & Throttle</h2>

[CodePen](https://codepen.io/hannahpun/pen/LqMjQR)

[https://mropengate.blogspot.com/2017/12/dom-debounce-throttle.html](https://mropengate.blogspot.com/2017/12/dom-debounce-throttle.html)

<br>

圖片來源： <a href="https://pngtree.com/free-backgrounds">free background photos from pngtree.com</a>