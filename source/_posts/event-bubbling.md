---
title: event bubbling & capturing(事件冒泡與捕獲) - JavaScript
date: 2019-03-10 21:00:00
tags:
  - javascript
categories:
  - javascript
---

本篇主要是透過線上課程：`udemy`、`w3school`及網路上搜尋資源所學習的。
## event bubbling & capturing

事件傳遞順序為：先捕獲，再冒泡。但是當事件傳到 target 本身後，是沒有分捕獲跟冒泡的。
事件捕獲：在網頁上點擊到一個元素，這個點擊的事件會先從`window`開始往下傳，一直傳到點擊到的元素。這個階段稱作捕獲階段。
事件冒泡：在事件捕獲到後，會一路傳回去`window`，這個時候稱作冒泡階段。

在上一篇所提到到 addEventListener 的事件監聽可以決定用事件冒泡或是事件捕獲來監聽這個 event。
`addEventListenter(‘click’,function(){...},false)`第三個欄位參數可以為 `true`，`false`。

- true: event capturing
- false: event bubbling

### event bubbling

下面的例子是用 body 去包住一個 box
設定為 false 後，點擊 box，事件會監聽按照從點擊位置往外傳到 window 的順序。
看起來只有點擊 box，但是因為監聽事件冒泡，所以會先 alert("hello box")然後再 alert("hello body")。

```html
<body class="body">
  <div class="box"></div>
</body>
```

```js
var elbox = document.querySelector(".box");
elbox.addEventListener(
  "click",
  function() {
    alert("hello box");
    console.log("box");
  },
  false
);

// event capturing
var elbody = document.querySelector(".body");
elbody.addEventListener(
  "click",
  function() {
    alert("hello body");
    console.log("body");
  },
  false
);
```

如果第三個欄位不輸入，也是 false，是 default 的設定值。

### event capturing

用跟 bubbling 一樣的例子，但是事件監聽將第三個欄位參數改為 true。
設定為 ture 後，點擊 box，事件只會監聽按照從 window 往內傳到點擊位置的順序。
點擊 box，因為監聽事件捕獲，所以會先 alert("hello body")然後再 alert("hello box")。

```js
var elbox = document.querySelector(".box");
elbox.addEventListener(
  "click",
  function() {
    alert("hello box");
    console.log("box");
  },
  true
);

// event capturing
var elbody = document.querySelector(".body");
elbody.addEventListener(
  "click",
  function() {
    alert("hello body");
    console.log("body");
  },
  true
);
```

### stop propagation

上述例子如果不想兩個都被點擊觸發到，只想單純執行自己就好，這時候就可以使用`stop propagation`來停止事件的傳遞。
依照`stop propagation`加在哪一階段，事件傳遞就會終止在該階段，不繼續傳遞下去。

這邊用 event bubbling 事件冒泡的例子。
box的操作中，在alert前就加上`e.stopPropagation()`來停止傳遞，所以這一段只會alert("hello box")，click的事件不會再往上傳到body去執行click事件了。


```js
var elbox = document.querySelector(".box");
elbox.addEventListener(
  "click",
  function(e) {
    //需要要給值代元素資訊
    e.stopPropagation(); //再去執行停止傳遞
    alert("hello box");
    console.log("box");
  },
  false
);

var elbody = document.querySelector(".body");
elbody.addEventListener(
  "click",
  function() {
    alert("hello body");
    console.log("body");
  },
  false
);
```

- [其他參考來源](https://blog.techbridge.cc/2017/07/15/javascript-event-propagation/)
