---
title: "[手刻] - 切版練習-Edge Ledger"
date: 2019-02-01 14:18:43
tags:
  - html
  - css
category:
  - project
---

這個網頁也是透過 Udemy 課程來學習，手刻練習，也有 RWD 的呈現。

## Demo

切版練習-Edge Ledger - [Demo](https://orow.github.io/MyProjects/EdgeLedger-CoursePractice/index.html)

## 1. Introduction 網頁介紹

這是一個投資帳務公司的網頁。
會固定在最上面的 navbar 並帶有透明度，showcase 背景圖加上 overlay 的暗黑色，文字敘述、按鈕與 what 區塊的捲動搭配，接著 what 區塊三等份的圖片與文字敘述，who 區塊則是一半照片，一半是描敘。clients 區塊則是用五張照片排版呈現，contact 區塊一邊為 form 表單，另一則是用引入 google mpas API 使用的地圖資訊。最後則是 footer。

RWD 則是設定在視窗寬度小於 768px 與大於 1100px 時會讀不同的 css 檔案。
字型則是引入 google font。
排版跟上一個手刻網頁的差異則是使用了`flexbox`的方式。

```html
<link rel="stylesheet" href="css/style.css" />
<link rel="stylesheet" media="screen and (max-width: 768px)" href="css/mobile.css"/>
<link rel="stylesheet" media="screen and (min-width: 1100px)" href="css/widescreen.css"/>
```

## 2. Layout Notes

這個網頁也是純手刻，所以 CSS 一開始會先 reset，將所有的 margin，padding 歸零，並先設定一些基本修飾，`box-sizing:border-box`則是讓 margin 與 border 等修改不會改變到 box model 大小。也預先引入了 google font 的字型

如果是練習的網頁，有著 sample，則會先看過一次，分析有哪些區塊，用到什麼方式，背景等。
這個網頁就有用到黑底白字，部分 item 在 active 時與 button 在 hover 時，呈現顏色不同都可以使用通用的 CSS 修飾。

`h1, h2, h3`等 title，可以先設定 padding 的方式，`a`的部分則先將`text-decoration:none;`設定好來移除底線，常見的`p`設定上下的 margin，這都可以視情況微調。

```css
@import url("https://fonts.googleapis.com/css?family=Roboto");

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: "Roboto", sans-serif;
  line-height: 1.4;
}

a {
  text-decoration: none;
}

p {
  margin: 0.75rem 0;
}
```

Navbar，左邊用了icon加上文字，右邊則是item，都分別用了`display:flex`來排版，就不需要用float的方式。位置固定則是用了`position:sticky`的方式，
![](https://i.imgur.com/VnEKc6p.png)

![](https://i.imgur.com/2L1aPMz.png)

RWD效果則是把flex-direction改為column
![](https://i.imgur.com/rT5dtXM.png)

**flexbox**的應用有額外一篇記錄：[css-flexbox](https://orow.github.io/2019/01/30/Flexbox/)

Showcasea區塊，背景圖則是高度用了100vh，設定為可視範圍的100％高度，接著包覆的區塊則用overlay，position用absolute，並且給了黑色透明度，文字敘述用了display，flex則改了方向`flex-direction:column`。
![](https://i.imgur.com/SQILh6x.png)

What區塊則是用flex來分三等分，帶icon與h3標題加上p文字敘述。
RWD則有三種，最寬的icon也有flex。
![](https://i.imgur.com/UldzTwb.png)

次寬，只有外層flex。
![](https://i.imgur.com/W03vgTF.png)

最小的畫面則是用display:block。
![](https://i.imgur.com/CYtZjAr.png)

Who區塊，左邊為圖片右邊為敘述，flex排版用了`display:flex`外，還使用了`flex:1`，這邊共有兩個flex:1，所以整個版面會各佔50%，文字敘述中的li則要加上`border-bottom`。
![](https://i.imgur.com/3nJxIR1.png)

RWD則是縮小後，圖片區塊display改為none。
![](https://i.imgur.com/c2xNmwC.png)

Clients區塊以flex來排版圖片，縮小時修改寬度，縮到最小時會把最後一個子元素以`display:none`來取消顯示最後一張圖片。
![](https://i.imgur.com/kd2N3mv.png)

![](https://i.imgur.com/ryiY7xx.png)

Contact區塊一邊為form表單，包含了input與textarea，另一邊是Google map(引入API)。
文章：[Google Map API](https://orow.github.io/2019/01/31/Google-Map-API/)
一樣是利用區塊flex後，下一層的個別區塊`flex:1`or`flex:2`來進行排版，縮小時則將方向改為column。
![](https://i.imgur.com/v9b4lRM.png)

![](https://i.imgur.com/e1budvn.png)

![](https://i.imgur.com/vd7KFGF.png)
