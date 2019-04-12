---
title: "[jQuery] - 練習-jQuery Content Slider"
date: 2019-03-14 20:18:43
tags:
  - jQuery
category:
  - project
---

## Demo

jQuery 練習-jQuery Content Slider- [Demo](https://orow.github.io/MyProjects/ProjectsInJS&jQuery/jQueryContentSlider/ContentSlider/index.html)

## 1. Introduction 介紹

這是在 Udemy 上課程的一個 slider 功能，網路上有很多這個功能的套件與框架，但還是把這個練習記錄下來。
整個 html 先用 div 包住 header 加上 prev 的圖片，再來是全部的 slide 資訊，最後才是 next 的圖片。Slide的圖片則是用了unsplash的隨機圖片，prev 跟 next 會用 float 來排版。
主要有 prev、next 還有隔一段時間就執行下一個 slide 的功能。
![](https://i.imgur.com/IeArKC4.png)

## 2. 功能

排版上來說第一個是 prev 圖片，再來是 slider 包住全部 slide 內容，最後是 next 圖片，這邊三個都要用`float:left`跟`position:relative`。
slider 中的每一個 slide 則是用`position:absolute`，slide 中的文字敘述區塊也是用 absolute，要貼齊上一層的 slide 的左下方則用`left:0`,`bottom:0`



JS部分：
宣告速度，自動播放需要設定速度，之後先加上一個active class給第一個slide的區塊（預設顯示）。
再讓全部slide區塊隱藏，之後抓取active class的區塊來顯示。
設定prev跟next點擊後、自動播放的handler。
設定next, prev slide的function。

宣告速度，還有自動播放，如果之後不想自動播放就改為false

```js
// Set Options
var speed = 500;    // fade spped
var autoswitch = true; //auto slider options
var autoswitch_speed = 4000 // auto slider speed
```

初始化active class，全部slide區塊隱藏，抓取active class進行顯示。

```js
// Add initial active class
$('.slide').first().addClass('active');

// Hide all slides
$('.slide').hide();

// Show first slide
$('.active').show();
```

設定prev與next點擊後、自動播放的handler的，每隔4000ms即自動執行nextSlide的function。

```js
// Next Handler 
$('#next').on('click',nextSlide);

// Prev Handler
$('#prev').on('click',prevSlide);

// Auto Slider Handler
if (autoswitch == true){
    setInterval(nextSlide,autoswitch_speed)
}
```

Prev & Next slide function設定：

Next需要先把正在顯示的slide移除active class並新增一個oldActive，這是用來判定是不是最後一個slide區塊。如果是最後一個，則把active加回第一個區塊，不是最後一個的話則加到下一個slide區塊。
判定後並新增active後則要把一開始的oldActive移除，全部slide要fadeOut，active要顯示的則fadeIn。

Prev則相同，只需要把判定的最後一個子元素改為第一個，next改為prev，first改為last即可。

```js
//Switch to next slide
function nextSlide(){
    $('.active').removeClass('active').addClass('oldActive');
    if($('.oldActive').is(':last-child')){
        $('.slide').first().addClass('active');
    }  else {
        $('.oldActive').next().addClass('active');
    }
    $('.oldActive').removeClass('oldActive');
    $('.slide').fadeOut(speed);
    $('.active').fadeIn(speed);
}

// Switch to prev slide
function prevSlide(){
    $('.active').removeClass('active').addClass('oldActive');
    if($('.oldActive').is(':first-child')){
        $('.slide').last().addClass('active');
    }  else {
        $('.oldActive').prev().addClass('active');
    }
    $('.oldActive').removeClass('oldActive');
    $('.slide').fadeOut(speed);
    $('.active').fadeIn(speed);
}
```

html程式：

```html
<div id="container">
  <header>
    <h1>jQuery Content Slider</h1>
  </header>
  <img src="img/arrow-left.png" alt="Prev" id="prev">
  <div id="slider">
    <div class="slide">
      <div class="slide-copy">
        <h2>Slider One</h2>
        <p>
          Lorem ipsum dolor sit, amet consectetur adipisicing elit. Quam amet
          illo doloribus debitis fuga quo, distinctio ipsam adipisci temporibus
          iste, sint odio saepe explicabo rerum incidunt quos nisi suscipit
          corrupti!
        </p>
      </div>
      <img src="https://source.unsplash.com/random/940x350">
    </div>
    <div class="slide">
      <div class="slide-copy">
        <h2>Slider Two</h2>
        <p>
          Lorem ipsum dolor sit, amet consectetur adipisicing elit. Quam amet
          illo doloribus debitis fuga quo, distinctio ipsam adipisci temporibus
          iste, sint odio saepe explicabo rerum incidunt quos nisi suscipit
          corrupti!
        </p>
      </div>
      <img src="https://source.unsplash.com/random/941x350">
    </div>
    <div class="slide">
      <div class="slide-copy">
        <h2>Slider Three</h2>
        <p>
          Lorem ipsum dolor sit, amet consectetur adipisicing elit. Quam amet
          illo doloribus debitis fuga quo, distinctio ipsam adipisci temporibus
          iste, sint odio saepe explicabo rerum incidunt quos nisi suscipit
          corrupti!
        </p>
      </div>
      <img src="https://source.unsplash.com/random/940x351">
    </div>
    <div class="slide">
      <div class="slide-copy">
        <h2>Slider Four</h2>
        <p>
          Lorem ipsum dolor sit, amet consectetur adipisicing elit. Quam amet
          illo doloribus debitis fuga quo, distinctio ipsam adipisci temporibus
          iste, sint odio saepe explicabo rerum incidunt quos nisi suscipit
          corrupti!
        </p>
      </div>
      <img src="https://source.unsplash.com/random/941x351">
    </div>
    <div class="slide">
      <div class="slide-copy">
        <h2>Slider Five</h2>
        <p>
          Lorem ipsum dolor sit, amet consectetur adipisicing elit. Quam amet
          illo doloribus debitis fuga quo, distinctio ipsam adipisci temporibus
          iste, sint odio saepe explicabo rerum incidunt quos nisi suscipit
          corrupti!
        </p>
      </div>
      <img src="https://source.unsplash.com/random/942x350">
    </div>
  </div>
  <img src="img/arrow-right.png" alt="Next" id="next">
</div>
```