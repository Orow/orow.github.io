---
title: "[手刻] - 切版練習-Hotel BT"
date: 2019-01-29 14:18:43
tags:
  - html
  - css
category:
  - project
---

透過 Udemy 課程來學習，這是一個比較起來第一個手刻比較完整的網頁，也有 RWD 的呈現。

## Demo

切版練習-Hotel BT - [Demo](https://orow.github.io/MyProjects/HBT-CoursePractice/index.html)

## 1. Introduction 網頁介紹

這是一個介紹 Hotel 的網頁，共分三個頁面：Home、About、Contact。
Home：基本 navbar，showcase 背景圖，文字敘述與 about page 連結按鈕，接著區塊一半顯示圖片一半顯示文字敘述並一樣含有按鈕連結到 about page，最後則是 footer。

About：相同的navbar，about區塊則是一半文字敘一半為圖片，接著是testimonial人物推薦，帶有背景圖，包覆兩個區塊並個含有圖片與文字敘述。

Contact：一樣的navbar，form表單包含兩個input一個textarea，搭配一個submit的按鈕，contact information則是三等份的font-awesome icon加上敘述。

RWD 則是設定在視窗寬度小於 768px 時會讀另一份 css 檔案。

```html
<link rel="stylesheet" media="screen and (max-width: 768px)" href="css/mobile.css"/>
```

## 2. Layout Notes

由於沒有用 bootstrap，是純手刻，在 CSS 一開始會先 reset，將所有的margin，padding歸零，`box-sizing:border-box`則是讓margin與border等修改不會改變到box model大小。

如果是練習的網頁，有著sample，則會先看過一次，分析有哪些區塊，用到什麼方式，背景等。
這個網頁就有用到黑底白字，部分item在active時與button在hover時，呈現顏色不同都可以使用通用的CSS修飾。

`h1, h2, h3`等title，可以先設定padding的方式，`a`的部分則先將`text-decoration:none;`設定好來移除底線，常見的`p`設定上下的margin，這都可以視情況微調。

```css
/* Reset */
*{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

/* Main Styling */
html,body{
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    line-height: 1.7em;

}
a{
    color: #333;
    text-decoration: none;
}

h1, h2, h3 {
    padding-bottom: 20px;
}

p{
    margin: 10px 0;
}

```

### 2.1 Home

Navbar區塊，左邊用上h1包著a連結到index.html首頁，CSS則需要設定底色，文字顏色等。
，另外一邊則用ul包住li當作navbar的清單，li則需要用`list-style:none`來將清單前的黑點移除，針對li中的a連結要用到`display:block`，文字置中。
這邊排版是用float的方式，針對左邊h1為`float:left`，右邊的ul則為`float:right`
![](https://i.imgur.com/6n0Ojxq.png)

RWD則是將左右的float排版都改為none，`float:none`

![](https://i.imgur.com/P1Y1TTP.png)


Showcase則是以有著整個區塊的背景圖片，加上h1標題，p敘述，a連結
背景用上url連結，不重複，置中覆蓋，並給在這區塊給一個高度。
接著下一階層設定文字顏色，文字置中等，也針對個別標題或文字微調`line-height`行距，還有a連結的button進行設定等。

這裏RWD修改到背景區塊的高度，從600px改為100%，視窗再縮小到設定寬度下後，則寬度會改為100%來符合容器大小。
![](https://i.imgur.com/yN94Kps.png)

接著hotel資訊區塊，左右也是float的left跟right進行排版，最外的區塊需要給高度，兩邊個給寬度50%，讓兩邊內容都站著網頁的一半寬。
![](https://i.imgur.com/ssuoTw8.png)

RWD則是把左邊的圖片設定`display:none`不顯示，右邊則是要把float改為none，寬度則需改為100%。
![](https://i.imgur.com/qHhsvmj.png)

features區塊則是`float:left`、`width:33.3%`來排版，內容則是font-awesome加上h3標題與p敘述。
![](https://i.imgur.com/For0viY.png)

RWD則是改為上下顯示，修改float與width：`float:none`、`width:100%`。
![](https://i.imgur.com/XjeRwFO.png)


### 2.2 About

Navbar則保留home頁面，只要修改已選擇後的顏色即可。
About區塊則是左邊為敘述右邊為圖片，左右區塊一樣用float來排版，width則是各給50%，圖片的部分則需要設定`display:block`以區塊顯示、寬度與`border-radius`。
![](https://i.imgur.com/tKCp6ng.png)

RWD則是上下顯示，float改為none，width改為100%。
![](https://i.imgur.com/6UwNrAt.png)

如果有左右的float顯示異常，需要在異常之後新增一個div區塊，針對這個區塊去用`clear:both`來消除異常顯示。

Testimonial這部分的背景圖是在最外的區塊加進去，`height:100%`，之後則為h2加上兩個個別包含圖片與敘述的區塊。區塊中左邊的圖片一樣為`float:left`排版，其他則一邊修改邊修改來微調。
![](https://i.imgur.com/wmVwo1M.png)

### 2.3 Contact

這個頁面分兩主要區塊，contact us的form區塊與contact information區塊。

Form表單上面先用h1標題與p文字敘述，然後form表單中個別用div包起來後個別修飾，每個div中都有label跟input，要注意到label要顯示成區塊`display:block`，才會換行。
Input與textarea寬度則是給100%，並加上border。textarea要額外給高度，一更新頁面才會有固定高度。
![](https://i.imgur.com/ke3bAEh.png)

Contact info區塊跟前面的類似，`float:left`、`width:33.3%`來排版，內容則為font-awesome icon、h3標題、p敘述，RWD則float改為none、width改為100%。

![](https://i.imgur.com/pN9nMEL.png)

![](https://i.imgur.com/KLHAJlS.png)
