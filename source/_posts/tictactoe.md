---
title: "[jQuery] - 練習-Tic Tac Toe"
date: 2019-03-21 21:18:43
tags:
  - jQuery
category:
  - project
---

## Demo

jQuery 練習-Tic Tac Toe- [Demo](https://orow.github.io/MyProjects/ProjectsInJS&jQuery/TicTacToeGame/index.html)

## 1. Introduction 介紹

這是 jQuery 操作 click 事件搭配新增 class 實現圈叉遊戲。
![](https://i.imgur.com/t5R6gcl.png)

- 需兩方輪流操作點擊後新增 class、插入兩方字串來顯示'o'與'x'
- 判定兩方誰達成連線
- 須判定和局、已觸發過點擊事件的欄位

## 2. 功能

先宣告圈與叉的變數字串、計算雙方輪流操作的變數等，主要都是用`if`、`else if`、`else`來判斷。
觸發 click 事件後，先判定圈是否有連成一線，再判定叉是否有連線，如果有連線都需要將全部欄新增的`o`、`x`、還有`disabled`以點擊過的 class 刪除，並將全部欄位的文字清空、計數 turns 歸 0。

```html
<div id="container">
  <header>
    <h1>Tic Tac Toe</h1>
  </header>
  <ul id="board">
    <li id="spot1">+</li>
    <li id="spot2">+</li>
    <li id="spot3">+</li>
    <li id="spot4">+</li>
    <li id="spot5">+</li>
    <li id="spot6">+</li>
    <li id="spot7">+</li>
    <li id="spot8">+</li>
    <li id="spot9">+</li>
  </ul>
  <div class="clearfix"></div>
  <footer>
    <button id="reset">Reset Game</button>
  </footer>
</div>
```

```js
// 以'o'為例，'x'則是在後面用 else if 新增第二個判斷式
$(document).ready(function() {
  var x = "x";
  var o = "o";
  var turns = 0;
  // Spot vars
  var spot1 = $("#spot1");
  var spot2 = $("#spot2");
  var spot3 = $("#spot3");
  var spot4 = $("#spot4");
  var spot5 = $("#spot5");
  var spot6 = $("#spot6");
  var spot7 = $("#spot7");
  var spot8 = $("#spot8");
  var spot9 = $("#spot9");

  $("#board li").on("click", function() {
    if (
      (spot1.hasClass("o") && spot2.hasClass("o") && spot3.hasClass("o")) ||
      (spot4.hasClass("o") && spot5.hasClass("o") && spot6.hasClass("o")) ||
      (spot7.hasClass("o") && spot8.hasClass("o") && spot9.hasClass("o")) ||
      (spot1.hasClass("o") && spot4.hasClass("o") && spot7.hasClass("o")) ||
      (spot2.hasClass("o") && spot5.hasClass("o") && spot8.hasClass("o")) ||
      (spot3.hasClass("o") && spot6.hasClass("o") && spot9.hasClass("o")) ||
      (spot3.hasClass("o") && spot5.hasClass("o") && spot7.hasClass("o")) ||
      (spot1.hasClass("o") && spot5.hasClass("o") && spot9.hasClass("o"))
    ) {
      alert("Winner: O");
      $("#board li").text("+");
      $("#board li").removeClass("disable");
      $("#board li").removeClass("o");
      $("#board li").removeClass("x");
    } else if ( '同上面'o'的判斷式...' ) {
      alert("Winner: X");
    //   同上'o'的reset行為
    }
});
});
```

和局判定，一樣需要 reset。
全部共有九個欄位，用一開始宣告用來輪流操作的變數，計算++後，值等於 9 即為和局。
![](https://i.imgur.com/cTtrry1.png)

```js
else if (turns == 9) {
    alert("Tie Game");
    $("#board li").text("+");
    $("#board li").removeClass("disable");
    $("#board li").removeClass("o");
    $("#board li").removeClass("x");
    turns = 0;
}
```

點擊過的欄位，如果再去點擊則不能執行
![](https://i.imgur.com/u0NYHtT.png)

```js
else if ($(this).hasClass("disable")) {
    alert("This spot is already filled");
}
```

輪流操作，點擊後新增 class，利用餘數除以 2 計算雙方操作，判斷成立後新增 class，每一次行為後都要判定是否連線。

```js
else if (turns % 2 == 0) {
    turns++;
    $(this).text(o);
    $(this).addClass("disable o");
    if ('ｏ連線判定') {
    alert("Winner: O");
    turns = 0;
    }
} else {
    turns++;
    $(this).text(x);
    $(this).addClass("disable x");
    if ('x連線判定') {
    alert("Winner: X");
    turns = 0;
    }
}
```

這邊則是獨立的 reset 按鈕，讓遊戲隨時可以重新開始。

```js
// Reset Handler
$("#reset").on("click", function() {
  $("#board li").text("+");
  $("#board li").removeClass("disable");
  $("#board li").removeClass("o");
  $("#board li").removeClass("x");
  turns = 0;
});
```
