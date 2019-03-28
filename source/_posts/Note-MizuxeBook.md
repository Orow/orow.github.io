---
title: 切版練習-MizuxeBook - Bootstrap
date: 2019-02-12 14:18:43
tags:
  - bootstrap
category: test
toc: true
---

## Demo

切版練習-MixuzeBook - [Demo](https://orow.github.io/MyProjects/BootstrapWith5Projects/MizuxeBook-Practice/index.html)

## 1. Introduction 網頁介紹

這是第一次嘗試用 bootstrap 來刻網頁，針對 RWD 的部分先不刻意進行。
這個書本網頁中 navbar 是固定在視窗最上面並且帶有透明度，視窗寬度縮小時，menu 列也會變成 collapse，點擊會有下拉顯示清單效果。
主要區域（showcase）有背景圖片並且有 overlay 覆蓋，內容則是一半是文字敘述，另一半則是圖片。
接下來則是 newsletter 區塊，是橫向排列的 input 與 button。
Boxes 區分為四個區塊有兩種不同的顯示
About 的這部分用到 accordion 的效果
Authors 則是用 card 來秀出不同 author 的介紹與 icon 連結
最後則是 contact form

## 2. Layout - Notes

Navbar 固定在最上層，用`position:sitcky`來達到這個效果，透明度則是用`opacity:0.9`。
視窗寬度縮小時會顯示如下圖的collapse。
button需要帶有`data-toggle='collapse'`, `data-target='#navbarNav'`，清單的div則需有`class='collapse navbar-collapse'`, `id=navbarNav`
**點擊前**
![](https://i.imgur.com/3Bz0s6U.png)
**點擊後**
![](https://i.imgur.com/Ta3A1qH.png)

collapse code: 

```html
<button class="navbar-toggler" data-toggle="collapse" data-target="#navbarNav">
  <span class="navbar-toggler-icon"></span>
</button>
<div class="collapse navbar-collapse" id="navbarNav">
  <ul class="navbar-nav ml-auto">
    <li class="nav-item">
      <a class="nav-link" href="#">Home</a>
    </li>
    <li class="nav-item">
      <a class="nav-link" href="#">About</a>
    </li>
    <li class="nav-item">
      <a class="nav-link" href="#">Meet The Authors</a>
    </li>
    <li class="nav-item">
      <a class="nav-link" href="#">Contact</a>
    </li>
  </ul>
</div>
```

Showcase 區域的 overlay 用的是`background: rgba(50, 146, 166, 0.8)`的效果。
這邊要注意的是 overlay 需要用`position:absolute; top:0; left:0`，上一層的元素則是需要用`position:relative`，這樣才會將 overlay 的位置才能夠固定。

Newsletter 三個部分則是分別都用 grid systems`col-4`這個 class 來劃分，input 的部分加上了`form-control, form-control-lg`;button 的部分也是加上了`btn, btn-lg`，還加上了`btn-block`讓 button 變成區塊元素來使用 grid。
![](https://i.imgur.com/ltej3tH.png)

Boxes 這邊就單純用了四個`col-3`，並且用`class='card'`來表達。
![](https://i.imgur.com/JYyHGjL.png)

About 用了 Accordion，點擊會有下拉與收起的效果。這是用 header 中的 a 連結來指導個別要下拉的區塊（給 id），需要`data-toggle='collapse'`等，相關內容從 bootstrap 官方文件可以找到。
裡面的 icon 則是用 font awesome 的方式。

點擊第一個 `id=collapse1`
![](https://i.imgur.com/XfQJ6ci.png)
點擊第二個 `id=collapse2`
![](https://i.imgur.com/xXGapiF.png)

Authors 用了跟 boxes 一樣的四個`col-3`，特別注意到圖片使用`margin-top:-50px`達到超出上緣的效果。
hover 的時候下方有三個用`<a></a>`連結包起來的 font awesome 的 icon。
![](https://i.imgur.com/1vWJ6sW.png)

這個 form 的表單中每個`input-group`的開頭，也就是左邊有個 icon，右邊則是一般的 input。
需要用`class='input-group-prepend`包起來，裡面再用`span class='input-group-text'`去包 icon。
以下圖紅框處說明：
![](https://i.imgur.com/TpGlD8i.png)

code:

```html
<div class="input-group input-group-lg mb-3">
  <div class="input-group-prepend">
    <span class="input-group-text"><i class="fas fa-user"></i></span>
  </div>
  <input class="form-control" type="text" placeholder="Name" />
</div>
```

## Question

1. Accordion 的包法要多多練習-collapse
2. Collapse 如果有多個,如何寫出個別點開,不會同時只能有一個 collapse 出現?
   Q. 找範例
3. Navbar 固定在網頁最上面,fixed 要加一個相同長寬的 div(尚未試過) or 用 margin-top 處理(在下一個 div);目前用 sticky
