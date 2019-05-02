---
title: "[JavaScript] - ES6（補充）"
date: 2019-04-24 12:00:00
tags:
  - javascript
  - ES6
categories:
  - javascript
---

本篇主要是透過線上課程：`HiSKIO`、`w3school`及網路上搜尋資源所學習的。

## ES6 概念補充

ES6 比 ES5 更提升程式品質，可以解決一些 ES5 遇到的問題，但是也會有兼容性的問題。
這邊可以使用[Babel](https://babeljs.io/)來轉換成舊的瀏覽器也可以支援的內容，`babel`是一種 js 的 compiler，不過還要搭配一些工具例如`Gulp`。不過我個人尚未用到，先把他紀錄下來。

這篇要補充的有四點

- 縮寫 shorthand
- 解構賦值 destructure
- 字串模板 string template
- 箭頭函式 arrow function

### 縮寫 Shorthand

物件縮寫
一個物件中如果 key 等於回傳的 value 就可以縮寫只寫出 key，程式會往上找到 function()中的參數。

```js
function makePoint(x, y) {
  return {
    x, // x: x,
    y // y: y,
  };
}
```

計算屬性
在宣告物件的時候，如果 key 是動態的，可以用[]中括號來刮起來，可以塞入變數、表達式等。

```js
function createObj(key, value) {
  // const obj = {};
  // obj[key]=value;
  const obj = {
    [key + 1]: value
  };
  return obj;
}
const person = createObj("name", "Jason");
// {
//     name1: 'Jason',
// }
const cat = creatObj("legs", "4");
// {
//     legs1 :4,
// }
```

函式縮寫

在物件中去宣告函式的時候，可以像是呼叫函式一樣，直接把 function 省略。

```js
const options = {
  name: "Options",
  level: 10,
  created: function() {...},
  // 不需要打: function
  mounted() {...},
  beforeCreate() {...}
};
```

### 解構賦值 Destructure

#### 陣列解構

取出陣列中第一個或第二個數值有更快速方便的方式。

```js
const nums = [1, 2, 3];

// 原本
const first = nums[0];
const second = nums[1];

// 陣列解構，與上面結果相同
const [first, second] = nums;

// 或是用let
let first;
let second;
[first, second] = nums;
```

預設值
如果超出原本陣列的長度，不存在的元素會變為 undefined，如果要進行計算，可以針對超出陣列的部分設定預設值。

```js
const nums = [10, 20];
const [first, second, third = 30] = nums;
```

忽略元素
只想取陣列中某一個元素

```js
const nums = [1, 2, 3, 4];
const [, second] = nums;
```

變數交換
原本做法需要在宣告額外一個變數來暫存數值，ES6 中可以直接陣列解構變數交換。

```js
let a = 1;
let b = 2;

// 原本
let tmp = a;
a = b;
b = tmp;

//解構賦值陣列交換
[a, b] = [b, a];
```

剩餘部分
想要取出除了第一個以外的數值，陣列中宣告`first`搭配`...others`

```js
const nums = [1, 2, 3, 4];
// 除了first以外都取出
const [first, ...others] = nums;
first === 1;
others; // [2,3,4];
```

#### 物件解構

與陣列解構幾乎相同，預設值，指派新名稱

```js
const point = {
  x: 100,
  y: 150
};
// 原本
const x = point.x;

// 物件解構
const { x, y } = point;

// 預設值
const { x, y, z = 0 } = point;

// 指派新名稱
// 原本
const px = point.x;

// 重新命名機制，x變為px，y變為py
const { x: px, y: py } = point;
```

#### 解構函式參數

```js
function distance(point) {
  // 原本
  return Math.sprt(point.x * point.x + point.y * point.y);
  // 解構
  const { x, y } = point;
  return Math.sprt(x * x + y * y);
}

// 解構
function distance({ x, y }) {
  return Math.sprt(x * x + y * y);
}
```

### 字串模板 String template

字串組合

```js
function greet(name) {
  console.log("Hello, " + name + "!");
  // 字串模板 string template - 反引號``插入表達式
  console.log(`Hello, ${name}!`);
}
// Expression 表達式
// Statement 陳述式
greet("Tom"); // Hello, Tom!

function greet(name, days) {
  console.log(`Hello, ${name}! It's been ${days * 24}hours.`);
  // 三元判斷式
  console.log(`Hello, ${name}! ${days < 7 ? "" : "Long time no see!"}`);
}
greet("Jack", 3);
```

多行字串
ES6 反引號中可以多行（換行）

```js
const words = `
    dddd
    dddddddddd
    dddddddddddd
`;
```

### 箭頭函式 Arrow function

#### 語法簡短

ES6 的箭頭函式省略了 ES5 的 function 這個關鍵字。
傳入的參數如果只有一個可以省略小括號。
如果函式本體只有一行，而且是 return，則可以直接接在箭頭後面。

```js
var double = function(x) {
  return x * 2;
};
// 箭頭函數
const double = x => {
  return x * 2;
};
// 參數如果只有一個可以省略小括號
const double = x => {
  return x * 2;
};
// 如果函式本體只有一行，而且是return可以接在箭頭後面
const double = x => x * 2;
```

array.map 例子

```js
// arr.amp
arr.map(function(elm, idx) {
  return elm + idx;
});

arr.map((elm, idx) => elm + idx);
```

監聽例子

```js
btn.addEventListener("click", () => console.log("Hi"));
```

#### 自動綁定

箭頭函式內部的 this 與外部相同，this 就是 context，函式的情境。

```js
const a = () => {
  console.log(this);
};
console.log(this);
```

一般的宣告方式則是情況

```js
// 視情況，例子在下面說明
var b = function() {
  console.log(this);
};
console.log(this);
```

this，作為物件的成員函式執行：this 就是該物件。

```js
// 直接執行：window(global)
var name = "Heisenburg";
var sayMyName = function() {
  console.log(this.name);
};

// 作為物件的成員函式執行：this就是該物件
var teacher = {
  name: "White"
};
teacher.sayMyName = sayMyName;

sayMyName(); // Heisenburg
teacher.sayMyName(); // White
```

button

```js
// <button id='btn' name='God damn right'>Response</button>
// 作為 dom 的監聽函式執行：this就是該 dom
btn.addEventListener("click", sayMyName);
// 執行sayMyName會印出God damn right
```

如果都用箭頭函式，this就都會是window下，this.name皆為Heisenburg。
