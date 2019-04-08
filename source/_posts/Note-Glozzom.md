---
title: "[Bootstrap] - 切版練習-Glozzom Multi Page"
date: 2019-03-03 14:18:43
tags:
  - bootstrap
category:
  - project
---

## Demo

切版練習-Glozzom Multi Page - [Demo](https://orow.github.io/MyProjects/BootstrapWith5Projects/Glozzom-Practice/index.html)

## 1. Introduction 網頁介紹

此版面有五個頁面： Home, About, Services, Blog, Contact。
Home 頁面用到 slider(bootstrap)、有兩個區塊的背景圖片不會隨著捲動而移動，用到了`modal`與`iframe`來顯示 youtube 的影片，用`lightbox`顯示照片。
About 頁面則是有一區塊背景圖不隨著捲動來移動，最後用的是`slick`的方式。
Service 頁面主要有一個區塊用 card 的方式去呈現（裡面有 list 與 font-awesome icon），最後用 accordion 呈現 FAQ。
Blog 頁面用了 bootstrap 的`card-columns`來排版。
最後的 contact 頁面則是一般的聯絡資訊，表單，還有 staff 的資料與照片呈現。

## 2. Layout Notes

### 2.1 Home

Showcase - Carousel slider，是整個背景圖片 slide，還有`indicator`, arrow 加上內容描述，arrow 要加上`slide prev & next`的功能，背景圖片這邊要注意,包著他的容器要給高度,若高度不是給容器則無法填滿。
接在下面的section 則是`font-awesome icon`加上文字敘述。
![](https://i.imgur.com/tNrLRgh.png)

下面兩處紅框區則是背景圖固定，不隨滑鼠捲動而移動，高度用`min-height`，可以透過`backgropund-position`來調整背景圖位置。
箭頭處則是`modal`。
![](https://i.imgur.com/KPPvFHq.png)

點擊後彈出視窗（iframe）
modal + iframe 參照範例與套件
![](https://i.imgur.com/5SBqVp6.png)

```html
<!-- Video modal -->
<div class="modal fade" id="videomodal">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-body">
        <button class="close" data-dismiss="modal">
          <span>&times;</span>
        </button>
        <iframe src="" frameborder="0" height="350" width="100%" allowfullscreen></iframe>
      </div>
    </div>
  </div>
</div>
```

```js
// Video Play
$(function() {
  // Auto play modal video
  $(".video").click(function() {
    var theModal = $(this).data("target"),
      videoSRC = $(this).attr("data-video"),
      videoSRCauto =
        videoSRC +
        "?modestbranding=1&rel=0&controls=0&showinfo=0&html5=1&autoplay=1";
    $(theModal + " iframe").attr("src", videoSRCauto);
    $(theModal + " button.close").click(function() {
      $(theModal + " iframe").attr("src", videoSRC);
    });
  });
});
```

Photo-gallery section: 用到 lightbox，照片我用 unsplash 隨機的照片。
lightbox 需要引用 CDN，[Lightbox for Bootstrap](http://ashleydw.github.io/lightbox/)
![](https://i.imgur.com/Ikwa2tv.png)

點擊會跳出額外視窗
![](https://i.imgur.com/KW8EbXR.png)

```js
// Lightbox Init
$(document).on("click", '[data-toggle="lightbox"]', function(event) {
  event.preventDefault();
  $(this).ekkoLightbox();
});
```

Newsletter section: inline-block 的 form
![](https://i.imgur.com/BE0VEsS.png)

### 2.2 About

Navbar: 相同，只有移動 active class 到該頁面的區塊，active 有 border-left 的效果。
接著的 Page header: 背景圖片仍是 fixed，不隨滑鼠捲動而移動。
About 區塊則是一邊敘述，一邊是圖片`class="rounded-circle"`有`border-radius:50%`的效果，再利用負的`margin-top`來超出邊界。
![](https://i.imgur.com/nPa1CgI.png)

Icon boxes: 含有 font-awesome 的 box，用 card 呈現。
Testimonial: 文字需要 slider(slick)，文字後面的 footer 要加上`blockquote-footer`的 class。
![](https://i.imgur.com/QhgC8sD.png)

slick 需引用 CDN，除了 js 檔外，也有 slick 的 css 檔案，[Slick](http://kenwheeler.github.io/slick/)
下面兩個方式都可以達成需求

```js
// // Slider function - single item -sample 1
$(".slider").slick();

// // Slider function - sample 2
$(".slider").slick({
  infinite: true,
  slidesToShow: 1,
  slidesToScroll: 1
});
```

### 2.3 Services

與 About 頁面類似，Services section: 用 card 呈現。
使用的是 Card 元件，方法很簡單，加上以下三個 class 即可，`.card-header.card-body.card-footer`
![](https://i.imgur.com/6nCcQRy.png)

Accordion: 要分成兩邊，一邊各三個 collapse，每個 collapse 需要有自己的 id 並用`a href`來連結。
![](https://i.imgur.com/dZIVgLO.png)

### 2.4 Blog

Blog section:
![](https://i.imgur.com/uBHf77f.png)

用 card-columns，這裡使用了 6 個 card，這裡的 card 有兩種：

- 有加 img, h4, small, p 等等
- blockquote: p, footer(small > cite)
  ![](https://i.imgur.com/0OurJto.png)

### 2.5 Contact

Contact section: 左邊為 information，右邊為 form-group 表單。
![](https://i.imgur.com/yBVd0MI.png)

staff section: 四個 staff，附上照片
![](https://i.imgur.com/ukWtLKv.png)
