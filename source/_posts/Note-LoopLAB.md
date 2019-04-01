---
title: 切版練習-LoopLAB - Bootstrap
date: 2019-02-13 14:18:43
tags:
  - bootstrap
category:
  - project
toc: true
---

## Demo

切版練習-LoopLAB - [Demo](https://orow.github.io/MyProjects/BootstrapWith5Projects/LoopLab-Practice/index.html)

## Introduction

## 1. Introduction 網頁介紹

這個網頁跟上一篇 MixuzeBook 類似， navbar 也是置頂視窗帶有透明度，也有 collapse。
主要區域有背景圖、overlay 覆蓋，但背景圖片不會隨著捲動跟著移動，內容則是一半是文字敘述，另一半則變為簡單註冊 form 表單。
接下來幾個區塊都是差不多的，一半文字敘述另一半是圖片，左右內容對調等等。
最後的 Footer 則是有一個 button，點擊會有 modal（彈出視窗）的效果。

## 2. Layout - Notes

Navbar 固定在最上層，這次使用`class='fixed-top'`來達到這個效果，加上`border-bottom: #008ed6 3px solid`進行底線的修飾，透明度則是用`opacity:0.8`。
collapse 則是與上一篇相同，網路上官方文件都可以搜尋的到。
ref:https://getbootstrap.com/docs/4.0/components/navbar/
![](https://i.imgur.com/dBY12AX.png)

Showcase 區域的 overlay 用的是`background: rgba(0, 0, 0, 0.7)`。
位置用`position:absolute; top:0; left:0`來固定。
內容紅框處用到了 flex 的效果，左邊 icon 用`class='align-self-start'`，右邊敘述則用`class='align-self-end'`
![](https://i.imgur.com/7pQvS29.png)
code:

```html
<div class="d-flex">
  <div class="align-self-start p-4">
    <i class="fas fa-check fa-2x"></i>
  </div>
  <div class="align-self-end p-4">
    Lorem ipsum dolor sit amet consectetur adipisicing elit. Aut cupiditate et,
    error neque quasi aspernatur.
  </div>
</div>
```

下面重複的區塊中圖片用了`class='rounded-circle'`來將圖片改變爲圓形。
效果等同 CSS 用`border-radius:50%`。
![](https://i.imgur.com/vyiCo56.png)

Footer 中的 button 用了`data-toggle="modal" data-target="#contactModal"`，target 是要呼叫內容的 id 名稱。
要顯示的內容用了`class='fade'`讓彈出與收起有fadeIn跟fadeOut的效果，用`display:none`讓他一開始不顯示。
相關套件在bootstrap的官方文件也可以找到資料。
ref: https://getbootstrap.com/docs/4.0/components/modal/
![](https://i.imgur.com/svzqigv.png)

modal code:

```html
<div class="modal fade text-dark" id="contactModal" style="display: none; padding-right: 14px;" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">Contact Us</h5>
        <button class="close" data-dismiss="modal">×</button>
      </div>
      <div class="modal-body">
        <form>
          <div class="form-group">
            <label for="name">Name</label>
            <input type="text" class="form-control" />
          </div>
          <div class="form-group">
            <label for="email">Email</label>
            <input type="email" class="form-control" />
          </div>
          <div class="form-group">
            <label for="message">Message</label>
            <textarea class="form-control"></textarea>
          </div>
        </form>
      </div>
      <div class="modal-footer">
        <button class="btn btn-primary btn-block">Submit</button>
      </div>
    </div>
  </div>
</div>
```

## Question

1. Navbar 的 menu 如何點選後字體顯示不同顏色？
   Q. 用:hover
