---
title: Closure(閉包) - JavaScript
date: 2019-03-06 20:00:00
tags:
  - javascript
categories:
  - javascript
---

## Closure(閉包)

JavaScript 允許巢狀函式（nesting of functions）並給予內部函式完全訪問（full access）所有變數、與外部函式定義的函式（還有所有外部函式內的變數與函式。
不過，外部函式並不能訪問內部函式的變數與函式。這保障了內部函式的變數安全。
來自[MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Functions)

這邊直接舉個網路上找到的例子說明

### [參考範例連結](https://pjchender.blogspot.com/2017/05/javascript-closure.html)

- 不使用 closure(閉包)
  同時計算狗與貓數量的函式

  ```js
  // 狗的計數函式
  var count = 0;

  function countDogs() {
    count += 1;
    console.log(count + " dog(s)");
  }

  // 中間是其他程式碼...

  // 貓的計數函式
  var count = 0;

  function countCats() {
    count += 1;
    console.log(count + " cat(s)");
  }

  countCats(); // 1 cat(s)
  countCats(); // 2 cat(s)
  countCats(); // 3 cat(s)
  ```

  看起來沒麼問題，但是當執行了 `countDogs()`跟 `conuntCats()`，會讓 `count` 增加。
  因為 count 是 `全域變數`，兩個函式執行時都會用到這個變數而變成重複計算。

  ```js
  var count = 0;

  function countDogs() {
    count += 1;
    console.log(count + " dog(s)");
  }

  // 中間是其他程式碼...

  var count = 0;

  function countCats() {
    count += 1;
    console.log(count + " cat(s)");
  }
  countCats(); // 1 cat(s)
  countCats(); // 2 cat(s)
  countCats(); // 3 cat(s)

  countDogs(); // 4 dog(s)，我希望是 1 dog(s)
  countDogs(); // 5 dog(s)，我希望是 2 dog(s)

  countCats(); // 6 cat(s)，我希望是 4 cat(s)
  ```

  Closure 就能解決這個問題

- 使用 closure
  利用閉包（closure）的作法，讓函式有自己私有變數，簡單來說就是 countDogs 裡面能有一個計算 dogs 的`count`變數；而 countCats 裡面也能有一個計算 cats 的`count`變數，兩者是不會互相干擾的。
  寫法如下：

  ```js
  function dogHouse() {
    var count = 0;
    function countDogs() {
      count += 1;
      console.log(count + " dogs");
    }
    return countDogs;
  }

  function catHouse() {
    var count = 0;
    function countCats() {
      count += 1;
      console.log(count + " cats");
    }
    return countCats;
  }

  const countDogs = dogHouse();
  const countCats = catHouse();

  countDogs(); // "1 dogs"
  countDogs(); // "2 dogs"
  countDogs(); // "3 dogs"

  countCats(); // "1 cats"
  countCats(); // "2 cats"

  countDogs(); // "4 dogs"
  ```

  這樣寫就把關於計算貓與狗個別的 count 關閉在 catHouse() 與 dogHouse() 中，當看到一個 function 中內 return 了另一個 function，通常就是有用到 closure。
  而在 dogHouse 這個函式中存在 count 這個變數，由於 JavaScript 變數會被縮限在函式的執行環境中，因此這個 count 的值只有在 dogHouse 裡面才能被取用，在 dogHouse 函式外是取用不到這個值的。
  最後因為我們要能夠執行在 dogHouse 中真正核心 countDogs() 這個函式，因此我們會在最後把這個函式給 return 出來，好讓我們可以在外面去呼叫到 dogHouse 裡面的這個 countDogs() 函式：
  ![](https://i.imgur.com/Acob83C.png)

  接著，當我們在使用閉包時，我們先把存在 dogHouse 裡面的 countDogs 拿出來用，並一樣命名為 countDogs（這裡變數名稱可以自己取），因此當我執行全域中的 countDogs 時，實際上會執行的是 dogHouse 裡面的 countDogs 函式：
  ![](https://i.imgur.com/T5UtXXz.png)

### 進一步的簡化程式
  如果熟悉在 closure 中會 return 一個 function 出來，可以不欲為裡面的函式命名，可以使用匿名函式的方式直接回傳出來。

  寫法：

  ```js
  function dogHouse() {
    var count = 0;
    // 把原本 countDogs 函式改成匿名函式直接放進來
    return function() {
      count += 1;
      console.log(count + " dogs");
    };
  }

  function catHouse() {
    var count = 0;
    // 把原本 countCats 函式改成匿名函式直接放進來
    return function() {
      count += 1;
      console.log(count + " cats");
    };
  }
  ```

  透過函式參數的方式把值帶入 closure 中，所以實際上只需要一個 counter，用不同的參數區分就可以記錄不同動物種類。

  ```js
  function createCounter(name) {
    var count = 0;
    return function() {
      count++;
      console.log(count + " " + name);
    };
  }

  const dogCounter = createCounter("dog");
  const catCounter = createCounter("cat");
  const birdCounter = createCounter("bird");

  dogCounter(); // 1 dog
  dogCounter(); // 2 dog
  catCounter(); // 1 cat
  catCounter(); // 2 cat
  birdCounter(); // 1 brid
  dogCounter(); // 3 dog
  catCounter(); // 3 cat
  ```
