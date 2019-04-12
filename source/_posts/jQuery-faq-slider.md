---
title: "[jQuery] - 練習-jQuery FAQ Slider"
date: 2019-03-18 20:18:43
tags:
  - jQuery
category:
  - project
---

## Demo

jQuery 練習-jQuery FAQ Slider- [Demo](https://orow.github.io/MyProjects/ProjectsInJS&jQuery/FAQAccordionSlider/index.html)

## 1. Introduction 介紹

這是在 Udemy 上課程的一個 accordion 的功能，網路上一樣也有這個功能的套件與框架，但還是把這個練習記錄下來。
顯示內容總共有六個區塊，每個區塊都是一個li中有照片跟文字再加上另一個accordion的文字內容共兩個li。
點擊的該區塊的li後，箭頭圖片要轉90度，下面另一個li文字敘述則要slidedown。

在點擊其他accordion的時候原先的要收起來，確保一次只有一個顯示。
![](https://i.imgur.com/msrCR6L.png)

## 2. 功能

在CSS中會先把`li class='a'`的部分用`display:none`隱藏起來，之後再透過jQuery操作將其slide顯示出來。點擊項目的時候，下一個元素（這邊會是`li class='a'`）會slide顯示，其他的同層元素則會slideUp隱藏起來。
點擊的時候也會把箭頭的圖片順時針轉90度，在CSS中針對rotate這個class用了`transform: rotate(90deg);`的效果，不是點擊的圖片則會移除rotate class。


html程式碼如下：
以兩個項目為例

```html
<ul class="faq">
  <li class="q">
      <img src="./img/arrow.png"> Lorem ipsum dolor sit amet consectetur adipisicing elit. Optio?
  </li>
  <li class="a">
      Lorem, ipsum dolor sit amet consectetur adipisicing elit. Ab praesentium at repellendus sapiente amet rem minus.
  </li>
  <li class="q">
      <img src="./img/arrow.png"> Lorem ipsum dolor sit amet consectetur adipisicing elit. Optio?
  </li>
  <li class="a">
      Lorem, ipsum dolor sit amet consectetur adipisicing elit. Ab praesentium at repellendus sapiente amet rem minus.
</ul>
```

JS部分：
宣告速度，'click'設為一個變數。
點擊了`li.q`之後，針對下一個元素進行slideToggle，原本是隱藏，現在就會slideDown顯示出來，而且其他的相同元素都會slideUp隱藏。

箭頭圖片則是對全部img判定不是現在點擊的`li.q的子元素中的img，就把rotate的class移除。
對現在點擊子元素的img去新增rotate子元素。

```js
// Accordion
var action = 'click';
var speed = '500';

$(document).ready(function(){
  $('li.q').on(action, function(){
      // Get next element
      $(this).next().slideToggle(speed)
      // Select all other answers
      .siblings('li.a').slideUp();

      // Get image for active question
      var img = $(this).children('img');
      // Remove the 'rotate' class for all images except the active
      $('img').not(img).removeClass('rotate');
      // Toggle rotate class
      img.toggleClass('rotate');
  });
});
```
