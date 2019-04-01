---
title: "[JavaScript] - event-addEventListener"
date: 2019-03-10 13:00:00
tags:
  - javascript
categories:
  - javascript
---

本篇主要是透過線上課程：`udemy`、`w3school`及網路上搜尋資源所學習的。

## Event

`Event`事件就是在操控瀏覽器時，會有點擊，滾動滑鼠，按下按鍵等都是事件的行為，當觸發這些行為時就會去執行寫好的程式行為。

- [w3school - event sample](https://www.w3schools.com/js/js_events_examples.asp)

## 事件監聽（addEventListener)

JavaScript 是一個事件驅動 (Event-driven) 的程式語言，當瀏覽器載入網頁開始讀取後，雖然馬上會讀取 JavaScript 事件相關的程式碼，但是必須等到「事件」被觸發(如使用者點擊、按下鍵盤等)後，才會再進行對應程式的執行。
透過事件監聽來監聽事件有沒有被觸發。

以下為監聽元素`class='btn'`有沒有被點擊到的監聽案例：

```js
var el = document.querySelector(".btn");
//'選擇事件','帶入匿名function','false'
el.addEventListener(
  "click",
  function(e) {
    alert("Hello");
  },
  false
);
```

### 事件綁定的差異

以點擊的案例來舉例，點擊可以透過`onclick = function(){...}`，
或是`addEventListener('click', function(){...},false)`來達成，但是兩者之間還是有差異。

在同時綁定多個事件時，onclick 只會顯示最後一個的事件，addEventListener 則是全部都會顯示。

```html
<input type="button" class="btnOn" value="on點擊" />
<input type="button" class="btnAdd" value="add點擊" />
```

```js
// onclick只會顯示最後一個的事件
var elOn = document.querySelector(".btnOn");
elOn.onclick = function() {
  alert("onclick-1");
};
elOn.onclick = function() {
  alert("onclick-2");
};

// addEventListener會兩個事件都觸發到
var elAdd = document.querySelector(".btnAdd");
elAdd.addEventListener(
  "click",
  function() {
    alert("add-1");
  },
  false
);
elAdd.addEventListener(
  "click",
  function() {
    alert("add-2");
  },
  false
);
```

### 帶出當下元素的資訊

用例子來說明，在 html 頁面建立一個 input button，用`onclick`事件來執行 function，在小括號中帶入一個參數，在大括號中再帶入的先前設定的參數。之後觸發事件後就可以印出當下元素各種資訊。
這個帶入參數的方式，在很多事件行為操作都需要利用到這個參數。

```html
<input type="buttom" value="click" class="btn" />
```

```js
var el = document.querySelector(".btn");
el.onclick = function(e) {
  console.log(e);
};
```

結果如下圖：
![](https://i.imgur.com/iK0h00X.png)

### preventDefault

`preventDefault()`可以取消元素的預設行為，比如說選取到的是`<a href="index.html">`連結，但並不想要點擊的時候回到 index.html 的頁面。

以`<a href="https://www.google.com.tw/">` 為例子。

```html
<a href="https://www.google.com.tw/" class="link">Orow Projects</a>
```

```js
var el = document.querySelector(".link");
el.addEventListener(
  "click",
  function(e) {
    // 取消默認的行爲
    // 這邊是取消點擊a連結後跳轉到指定頁面
    e.preventDefault();
  },
  false
);
```

或者選取到 form 表單裡面`submit`的按鈕，不想要點擊後馬上送出資料到後端，想要先在 local 端先進行基礎驗證的時候就可以使用這個`preventDefault`的方式。
取消預設行為後，javascript 中會用 post 的方式來傳送資料到後端。

### target - 目前在網頁上的位置
舉一個例子來說，想要知道滑鼠現在是點擊到哪一個位置的時候，就會用`.target`的方式。
當然`.target`裡面還有很多資訊可以利用，這邊就課程上一開始提到的方式來舉例。

```html
<div class="header">
  <ul style="padding-top:100px;border:1px solid #333">
    <li>
      <a href="#">1111</a>
    </li>
  </ul>
</div>
```

```js
var el = document.querySelector(".header");
el.addEventListener(
  "click",
  function(e) {
    console.log(e.target);
    // target中還有各種語法,如nodeName是大寫的節點名稱
  },
  false
);
```

上面的方式在網頁上點擊過後，console.log就會印出是`ul`，`li`或是`a`。
