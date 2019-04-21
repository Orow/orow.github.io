---
title: "[JavaScript] - let & const(ES6)"
date: 2019-03-12 21:00:00
tags:
  - javascript
categories:
  - javascript
---

本篇主要是透過線上課程：`udemy`、`w3school`及網路上搜尋資源所學習的。

## ES6 入門: let & const - JavaScript

ES6 比 ES5 更提升程式品質，可以解決一些 ES5 遇到的問題，但是也會有兼容性的問題。
這邊可以使用[Babel](https://babeljs.io/)來轉換成舊的瀏覽器也可以支援的內容，`babel`是一種 js 得 compiler，不過還要搭配一些工具例如`Gulp`。不過我個人尚未用到，先把他紀錄下來。


### window var 的特性

在 javascript 宣告變數的時候，會盡量避免去污染到全域變數。

```js
var a = 1;

for (var i = 0; i < 3; i++) {
  console.log(i);
}
```

上面的方式用`var a = 1`,會讓 a 變成全域變數，`for (var i = 0 ; i < 3 ; i++)`，i 也會變成全域變數(i=3)。這樣容易導致程式間互相污染全域變數
這時候用 let, const 可以解決這種狀況。

### let

例子 1：在 function 中用 let 來宣告變數，雖然變數跟外面的全域變數都是 a，但並不會互相影響。

```js
var a = 0;
function changeA() {
  let a = 0;
  a = 1;
  console.log(a);
}
changeA();
console.log(a);
```

例子 2：let 與 for 迴圈的用法

```html
<ul class="list">
  <li>1</li>
  <li>2</li>
  <li>3</li>
</ul>
```

```js
// 不會污染到全域變數(global)
for (let i = 0; i < 3; i++) {
  console.log(i);
}

// 點擊list就alert的list編號
// 污染到i,結果i都是=3
const listLen = document.querySelectorAll(".list li").length;
for (var i = 0; i < listLen; i++) {
  document.querySelectorAll(".list li")[i].addEventListener("click", function() {
      alert(i + 1);
    });
}

// 改用let即可
const listLen = document.querySelectorAll(".list li").length;
for (let i = 0; i < listLen; i++) {
  document.querySelectorAll(".list li")[i].addEventListener("click", function() {
      alert(i + 1);
    });
}
```

### const

特性：唯讀，不能被修改。
const 多數用在當這個變數不想被修改，或者是 url 網址等。

例子：

```js
let a = 1;
a = 2;
console.log(a); // a 會被修改成為2

const b = 1;
b = 2;
console.log(b); // Uncaught SyntaxError: Assignment to constant variable.
```

但如果`const`是用在 object 或是 array 的話是可以更改的。

```js
// object, array還是可以被修改
const obj = {
  url: "http://www.x.com"
};
obj.url = "x";
console.log(obj); // obj = {'x'}
```

如果想在 const = object or array 仍然不能被修改的話，需要先加上 freeze 的方式。
這邊用 object 來當作範例。

```js
// 如果想要object, array都不能被修改
const obj = {
  url: "http://www.x.com"
};
// 用freeze來凍結
Object.freeze(obj);
obj.url = "x";
console.log(obj); // obj = {url: "http://www.x.com"}
```

### let, const注意事項
- 用let與const在宣告變數的時候，在哪一行宣告就是建立在哪一行。
    如果是用var的話則會提升（hoisting)，但是只有提升宣告變數，而不包含賦予值的部分

    ```js
    // 1st a = undefined(不是找不到), 2nd a = 3
    console.log(a);
    var a = 3;
    console.log(a);
    ```


    ```js
    // // 改用let(const一樣)
    // // 1st a = 找不到, 2nd a = 3
    console.log(a);
    let a = 3;
    console.log(a);
    ```

- 在相同區塊中，不能二次命名

    ```js
    var a = 1;
    var a = 2;

    // let, const 這樣寫會顯示b已經被宣告 
    let b = 5;
    let b = 6;
    // Uncaught SyntaxError: Identifier 'b' has already been declared
    ```

- let, const不會在全域變數（window)中
