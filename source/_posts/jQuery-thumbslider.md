---
title: "[jQuery] - 練習-jQuery Thumbslider"
date: 2019-03-19 20:18:43
tags:
  - jQuery
category:
  - project
---

## Demo

jQuery 練習-jQuery Thumbslider- [Demo](https://orow.github.io/MyProjects/ProjectsInJS&jQuery/ApplyStyleThumbslider/index.html)

## 1. Introduction 介紹

這個 slider 跟之前的 jQuery Content Slider 練習的效果是不一樣的，content slider 是利用 show 跟 hide 來隱藏與顯示，這個 slider 則是 carousel，把所有圖片都用水平排版，利用 float 的方式並將全部照片寬度加總來製造左右滑動的效果，在 bootstrap 切版練習中也有套件可以直接使用。

排版的部分，從外觀上來說由上而下為：標題、照片區塊、indicator 區塊。
網頁功能中最上層與最下層的淺灰色則是用背景圖、加上背景位置來呈現。
照片區塊設定了長度與高度，裡面的每一個 img 也都會設定寬度與高度讓程式可以去獲取寬度來操作水平滑動，並讓版面更整齊。
indicator 區塊，透過點擊可以直接滑動到位置上的圖片，用上背景圖覆蓋用來表示點擊後的狀態，此網頁也帶有自動照片滑動的效果。
![](https://i.imgur.com/NIY8W6V.png)

## 2. 功能

外觀上最上與最下面有淺灰色的區塊是用背景圖達到的效果，上方的淺灰是圖片放在標題區塊中並給高度，下方的淺灰的圖片則是放 標題區塊的上一層，也就是最外層的 container 中、位置則是設定在 buttom，針對背景顏色設定為白色，來讓中間區塊的底色呈現白色，另外兩者都用 repeat-x 針對 x 方向都會重複同一張照片。

主要區塊功能：圖片的點擊水平滑動、自動水平滑動。

jQuery 中主要分下面幾個部分

- 陣列加總圖片總寬度
- 點擊縮圖的圖片滑動，水平滑動-寬度值的計算
- 自動水平滑動

先放上 html 程式碼方便對照

```html
<div id="slides">
  <div class="slide">
    <img src="./img/slides/device-imac.png" width="900" height="200" />
  </div>
  <div class="slide">
    <img src="./img/slides/device-macbook.png" width="900" height="200" />
  </div>
  <div class="slide">
    <img src="./img/slides/device-iphone.png" width="900" height="200" />
  </div>
  <div class="slide">
    <img src="./img/slides/device-ipad.png" width="900" height="200" />
  </div>
</div>
<nav id="menu">
  <ul>
    <li class="sep"></li>
    <li class="product">
      <a href="">
        <img src="./img/slides/device-imac-thumb.png" alt="thumbsnail" />
      </a>
    </li>
    <li class="product">
      <a href="">
        <img src="./img/slides/device-macbook-thumb.png" alt="thumbsnail" />
      </a>
    </li>
    <li class="product">
      <a href="">
        <img src="./img/slides/device-iphone-thumb.png" alt="thumbsnail" />
      </a>
    </li>
    <li class="product">
      <a href="">
        <img src="./img/slides/device-ipad-thumb.png" alt="thumbsnail" />
      </a>
    </li>
  </ul>
</nav>
```

### 2.1 陣列加總圖片總寬度

先宣告總長度為 0，準備要從 0 要加上寬度，再新增一個空的陣列。
再來 each 取得每一張照片的寬度並加總，並加上一個確認的機制，以確保圖片 html 標籤中有加上 width 的屬性。
再把總寬度設定在整個 slider 區塊中。

```js
// Declare vars
var totalWidth = 0;
var positions = new Array();

$("#slides .slide").each(function(i) {
  // Get slider widths, 用float:left=>所以變水平方向
  // 四張圖是水平slide,是用寬度來算
  positions[i] = totalWidth;
  totalWidth += $(this).width(); // positions =[0, 900, 1800, 2700]
  // totalWidth=2700

  // Check widths
  if (!$(this).width()) {
    alert("Please add a width to your images");
    return false;
  }
});
// Set width
$("#slides").width(totalWidth);
```

### 2.2 點擊縮圖的圖片跳轉，水平滑動-寬度值的計算

在圖片下方也使用了像 indicator 的 list 的概念，透過點擊可以直接滑動到位置上的圖片。
利用新增 active class 來加在要顯示的 li 標籤中，CSS 設定 active 時有圖片覆蓋的效果。
在 click 的 function 中先將全部帶有`class='product'`的 li 都移除 active 的 class，再針對點擊的 a 連結的父元素加上 active class。

先前已經把 position 陣列取出`position=[0, 900, 1800, 2700]`，這邊要使用點擊 a 連結"之前"的父元素 li 的長度，再帶入 position 陣列中等於 index 的概念，以此為概念並搭配 margin-left，用負值的 margin-left 才會使區塊往右移動，用上 animate 效果移動到 position index 的 px 寬度位置。

因為是透過點擊來捲動圖片，所以要透過clearInterval來清除自動播放的間隔時間。

```js
// Menu item click handler
$("#menu ul li a").click(function(e) {
  // Remove active class and add inactive class
  $("li.product").removeClass("active");
  // Add active class to parent(li)
  $(this).parent().addClass("active");

  // 長度：點擊a的父元素"之前的所有class=producta"的元素
  var pos = $(this).parent().prevAll(".product").length;

  // 負值的margin-left才會使區塊往右移動
  $("#slides").stop().animate({ marginLeft: -positions[pos] + "px" }, 450);
  // Prevent default - a 的跳轉連結
  e.preventDefault();

  // Stop autoScroll, 從click當下那個元素開始重新計算interval時間
  if (!autoScroll) clearInterval(itvl);
});
```

### 2.3 自動水平滑動

自動水平滑動功能是用setInterval設定執行間隔的方式來達成，間隔執行則是用餘數計算加上eq的方法來用trigger點擊事件。
自動水平滑動是從第一張開始滑到第二張圖片，記得要把第一個 li class=product 先加入 active class。

宣告的變數current是用在餘數計算，一開始讓current=1，分母`$("#menu ul li a")`是帶有四個物件的陣列長度，餘數計算後結果=1，`.eq()`用的是index的方式，所以.eq(1)會等於操作元素陣列中的第二個，意思就是針對第二個a連結去trigger點擊事件已達到捲動圖片的效果。
執行點擊事件後再用`current++`，搭配分母陣列長度為4，所以計算的eq值都會是1,2,3,0來重複執行。

設定間隔執行時間，每隔10秒鐘要執行一次autoScorll的function來水平滑動。


```js
// Make first image active
$("#menu ul li.product:first").addClass("active");

// Auto Scroll function
var current = 1;
function autoScroll() {
  if (current == -1) {
    return false;
  }
  // .eq(1,2,3,0)依序的值 => 第一個a要執行a陣列[1]的click就會滑到a[1],以此類推
  $("#menu ul li a").eq(current % $("#menu ul li a").length).trigger("click", [true]);
  current++;
}

// Duration for autoScroll
var duration = 10;
var itvl = setInterval(function() {
  autoScroll();
}, duration * 1000);
```
