---
title: "[jQuery] - 練習-Photos Gallery View"
date: 2019-03-21 14:18:43
tags:
  - jQuery
category:
  - project
---

## Demo

jQuery 練習-Photos Gallery View- [Demo](https://orow.github.io/MyProjects/ProjectsInJS&jQuery/jQueryPortfolioGallery/index.html)

## 1. Introduction 介紹

這是顯示照片集的網頁，滑鼠移動到圖片上覆蓋會有標題與文字敘述搭配背景覆蓋，上方的 navbar item 點擊則可針對圖片顯示做篩選。
![](https://i.imgur.com/pzOiM8V.png)

引入 lightbox，點擊圖片會彈出視窗，且會顯示照片總數與目前張數。
![](https://i.imgur.com/VisJwGa.png)

這邊使用的 lightbox by Lokesh Dhakar：[Lightbox](https://lokeshdhakar.com/projects/lightbox2/#getting-started)

## 2. 功能

一般的網頁架構，有 navbar、標題區塊、主要區塊是圖片顯示。
主要功能

- 判斷 class 名稱，篩選後顯示圖片
- 滑鼠`mouseenter`、`mouseleave`搭配文字背景覆蓋的效果
- 引入 lightbox

navbar 的 item 用來篩選照片，每一張照片的 class 要先設定好名稱，再用 jQuery 來判斷 class 名稱來顯示。

點擊 navbar item 中的 a 連結後，直接取用 navbar item 名稱來篩選每一張圖片的 class 名稱，這邊用了`toLowerCase()`把文字轉為小寫、`replace`把空格轉為`-`。點擊`All Projects`會把所有 hidden class 的 li 全部 fadeIn 顯示、並把 hidden class 移除。

如果點擊的是其他的項目則會把全部 li 的陣列用 each 來判斷是不是符合點擊項目的名稱後再去操作`hide`與`fadeIn`
![](https://i.imgur.com/mZduSwt.png)

程式碼擷取一個 li 範例，li class 可以輸入單個或多個名稱，點擊後篩選就依照是否有該 class 來執行。

```html
<header>
  <div class="container">
    <h1 class="logo">P GALLERY</h1>
    <!-- Navbar Item -->
    <nav>
      <ul>
        <li><a href="#">All Projects</a></li>
        <li><a href="#">Design</a></li>
        <li><a href="#">Programming</a></li>
        <li><a href="#">CMS</a></li>
      </ul>
    </nav>
  </div>
</header>

<!-- 主要照片區塊 -->
<!-- 舉一個li說明 -->
<section>
  <div class="container">
    <h1 id="heading">All Projects</h1>
    <ul id="gallery">
      <li class="design">
        <a href="./img/web1.jpg" data-lightbox="Projects" data-title="Project 1" data-desc="Lorem ipsum dolor sit amet consectetur adipisicing elit. Fugiat rerum modi harum.">
        <img src="./img/web1.jpg" />
        </a>
      </li>
</section>
```

```js
$("nav a").on("click", function() {
  // current class assignment
  $("nav li.current").removeClass("current");
  $(this).parent().addClass("current");

  // Set Heading text
  $("h1#heading").text($(this).text());

  // Get & Filter link text
  var category = $(this).text().toLowerCase().replace(" ", "-");

  // Remove hidden class if 'all-projects' is selected
  if (category == "all-projects") {
    $("ul#gallery li:hidden").fadeIn("slow").removeClass("hidden");
  } else {
    $("ul#gallery li").each(function() {
      if (!$(this).hasClass(category)) {
        $(this).hide().addClass("hidden");
      } else {
        $(this).fadeIn("slow").removeClass("hidden");
      }
    });
  }
  // Stop link behaviour
  return false;
});
```

另外則是滑鼠移動到圖片上`Mouseenter`會有標題與文字搭配背景的覆蓋效果，移開`mouseleave`則恢復圖片顯示。
觸發 Mouseenter 事件後，取用自訂屬性的 data-title、data-desc，之後 append 一個 overlay 的區塊在觸發的 li 之後，再將取得自訂屬性的標題與描述內容加進去，再執行 fadeIn 效果。

Mouseleave 則是會先用`stop()`停止在執行的效果後再去`fadeOut`

![](https://i.imgur.com/VmGWzdn.png)

```js
// Mouseenter overlay
$("ul#gallery li").on("mouseenter", function() {
  // Get data attritube values
  var title = $(this).children().data("title");
  var desc = $(this).children().data("desc");

  // Validation
  if (desc == null) {
    desc = "Click To Enlarge ";
  }
  if (title == null) {
    title = "";
  }

  // Create overlay div
  $(this).append('<div class="overlay"></div>');
  // Get the overlay div
  var overlay = $(this).children(".overlay");

  // Add html to overaly;
  overlay.html("<h3>" + title + "</h3><p>" + desc + "</p>");

  // Fade in overlay
  overlay.fadeIn(800);
});

// Mouseleave overlay
$("ul#gallery li").on("mouseleave", function() {
  // Get the overlay div
  var overlay = $(this).children(".overlay");

  // Fade out overlay & Remove overlay
  overlay.stop().fadeOut(500, function() {
    $(this).remove();
  });
});
```

引入lightbox，這個範例是直接用引入script跟css檔案，並按照提供的方式來設定即可。
在a連結的屬性中加上`data-lightbox`、`data-title`、`data-desc`等等。
![](https://i.imgur.com/Jwlyl6m.png)

![](https://i.imgur.com/s95w98w.png)
