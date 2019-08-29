---
title: "[jQuery] - Basic"
date: 2019-02-27 20:00:00
tags:
  - jQuery
categories:
  - jQuery
---

新手針對網路課程與資料做筆記記錄。
在剛接觸時加深自己印象與記憶點。
網路課程：`freeCodeCamp`, `Udemy`, `pluralsight`

## jQuery 基礎概念

jQuery 是 JavaScript 的函式庫(library)，用來快速的選取元素並執行一些行為，還有加強了 AJAX 以及 Event 的功能，也可以透過外掛程式來擴充功能，簡單來說就是讓開法者更輕鬆方便的去製作網站。

### 概論

- find - elements in an HTML document
- change - HTML content
- listen - to what a user does and react accordingly
- animate - content on the page
- talk - over the network to fetch new content

選擇 DOM 中元素之後修改內容，依照使用者所需要的去監聽已經選取的元素，並可以製作動畫的效果。並在網路上與各方資訊傳輸。

## 選取元素

### Selector

`$` 是`jQuery`的縮寫，這個 dollar sign 是 jQuery 的物件，使用這個就是用 jQuery 來選取元素。
舉例來說：
DOM 的操作上相比 JavaScript 的`getElementById()`、`querySelector()`等，jQuery 可以用更方便，像是 CSS selector 的方式來選取元素，舉幾個例子來說明：

- `$(div)`就是選取文件內所有`<div>`的元素。

- `$('#navbar')`是選取`id='navbar'`的元素。

- `$('.title')`則是選取`class='title'`的元素。

- `$('div.contents p')`就是選取`<div class='contents>`中所有的`<p>`節點。

- `$('div > li')`，`>`是 child 的意思，這邊只會選取`div`下一層中的`li`元素。

### Traversing

上面提到的 selector 是從根元素去尋找其下面的子元素，`traversing`則是用篩選的方式來做這些過濾、查訪元素。
舉幾個例子說明：

- `eq()` - 取得第 index 個元素（index 從 0 開始算）。
  `$('p').eq(3)`：選取第四個 p 元素。
  相比`get()`得到的是 DOM 物件，`eq()`是取的 jQuery 物件。

- `filter()` - 針對本身集合符合條件的元素（可用逗號分開多個選擇）。
  `$('p').filter('.content')`：取得`<p class='content'>`的段落。

- `find()` - 針對其子集合符合條件的元素。
  `$('p').find('.content')`：取得在`<p>`中有任何 class 等於`content`的段落。

- `parent()` - 取得上一層的父元素。

- `next()` - 取得下一個元素（同層）。

- `siblings()` - 取得所有同層元素。

- `each()` - 用來針對對象重複的執行函式。類似 for 的 iteration

參考 jQuery 官方文件[Traversing](https://api.jquery.com/category/traversing/)

## 操作行為

### Manipulation

jQuery 可以操作 DOM 來改變元素屬性，內容與修改 CSS 樣式等。

- `text()` - 改變純文字內容，類似 JS 的 textContent。

- `html()` - 改變元素內容，類似 JS 的 innerHTML。

- `append()` - 在每個匹配的元素內部最後面加入內容。

- `after()` - 在每個匹配的元素的後面加入內容。

  ```js
  // html
  <p>I would like to say: </p>

  // append
  $('p').append('<b>Hello</b>');
  // 得到的結果
  [<p>I would like to say: <b>Hello</b></p>]

  // after
  $('p').after('<b>Hello</b>');
  // 得到的結果
  [<p>I would like to say: </p><b>Hello</b>]
  ```

- `val()` - 取得所選的元素的 value。

- `addClass()` - 把()中的 class 名稱加到所選的元素中。

- `attr()` - 針對選取的元素取出屬性值，也可以修改屬性值
  ex.

      ```js
      $("a:first").attr("href")
      // a href="index.html"
      $("a:first").attr("href", "contact.html")
      // a href="contact.html"
      ```

- `css()` - 取出 css 樣式或是者直接修改樣式。
  ex. 將 div 的背景顏色修改為紅色

      ```js
      $( "div" ).click(function() {
      $(this).css( "background-color", "red" );
      });
      ```

更多操作可以參考 jQuery 官方文件[Manipulation](https://api.jquery.com/category/manipulation/)

### Effects

jQuery 效果，都是很直覺的名稱，參照官方文件的用法即可。

- `show()`,`.show([duration][,complete])` - 顯示內容，也可以顯示後再執行 funcition，速度設定有 default(400ms)，也可以自行設定 ms 或是 fast(600ms)、slow(200ms)。
  ex.

      ```js
      $("button").click(function() {
      $("p").show("slow");
      });
      ```

- `slidedown()` - 下拉的效果、用法與 show 類似。

- `animate()` - 用法：`animate(properties[,duration] [,easing] [,complete])`, `animate(properties, options)`，在 properties 設定想要的 CSS 結果，過程與時間 jQuery 是有預設值，也可以自己設定。
  ex.

  ```js
  $("#clickme").click(funciton(){
      $("#book").animate({
          opacity: 0.25,
          left: "+=50",
          height:"toggle"
      }, 5000, function(){
          //Animation complete
      });
  });
  ```

更多效果可以參考 jQuery 官方文件[Effects](https://api.jquery.com/category/effects/)

### Events

jQuery 有事件觸發處理，或者添加函數到被選元素的事件處理。

- `focus()` - 觸發焦點的事件。

- `blur()` - 觸發失去焦點的事件。

- `click()` - 觸發點擊事件。

- `change()` - 當所選則的區域中有發生變話時觸發。

- `mouseover()` - 當滑鼠移動到選取區域時觸發。

更多事件可以參考 jQuery 官方文件[Events](http://api.jquery.com/category/events/)

## document ready

想要在整個頁面資料讀取完畢後再去透過 jQuery 執行 JS 時，就會使用這個方式。
`$(document).ready(function(){...}`，一開始先執行 document.ready 後才進去執行 function。

### 與 window.load 的差異

- document ready
  會在網頁下載完可以正確取得後就立刻執行，不包含圖檔等。
- window.onload
  window 的 load 的事件則會等到整個網頁的所有資源都下載完畢才會開始執行，如果有 100 張圖片就會等到下載完後才去執行。
  也沒有辦法多次指定不同函數來執行，後指定的函數會覆蓋先前的。
    ex. 只會執行最後的

    ```js
    window.onload = function() {
    alert("Hello world!");
    };
    window.onload = function() {
    alert("您好，歡迎來到 jsGears.com ～");
    };
    ```

[參考來源1](http://jsgears.com/thread-63-1-1.html)
[參考來源2](https://blog.miniasp.com/post/2010/07/24/jQuery-ready-vs-load-vs-window-onload-event)