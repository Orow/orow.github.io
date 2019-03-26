---
title: Start to learn HTML & CSS
date: 2019-01-10 14:18:43
tags: html css
category: test
---

# HTML & CSS - 新手村木人樁

## 從零開始 From Scratch

*Mentor：要踏入網頁需要先了解HTML與CSS是什麼*

* HTML(mark up) - 標籤式語言，網頁的結構與內容，會是使用者所看到，瀏覽頁面所接收到的信息
* CSS(style) - 外觀的控制，用來修飾網頁，字體、顏色、背景、邊框等等

> 在什麼都不會,沒有基礎的狀況下，直接從**做中學**開始

## freeCodeCamp
這是一個線上學習的課程，依照網站上每個小單元的過關條件進行coding，如果不會寫卡住，也有提示可以參考，且是直接在網頁上coding，這對於新手沒有接觸過程式、編輯器安裝等，個人是覺得很友善的。而且是免費的！！  
想試試看請點擊: [freeCodeCamp](https://learn.freecodecamp.org/)

### 最早接觸到的html
1. h1,h2等標題tag
2. 段落paragraph - p
3. comment to HTML
4. 插入image
5. a link
    * href='url'     - 點擊後可以直接跳到該網址
    * href='#footer' - 點擊後可以直接跳到元素中id="footer"的位置
    * href='#' - 點擊會回到網頁最上端
    * target="_blank" - 點擊後是另開視窗
6. ul / ol - unordered list & ordered list
    * 都會接著包li
    * ul是圓點顯示，li用編號顯示
7. input
    * type='text' - 可輸入內容
    * placeholder - 該欄位在為輸入前顯示的內容
    * required - 放在form如果submit出去時，沒有輸入內容會被擋下
8. form
    * action="" - submit後送到server的位址
9. button
    * type="submit" - 放在form內,點擊就會執行submit
10. radio buuton
    * input type='radio'
    * name 彼此有關聯要輸入並用-隔開，否則會可以複選
    * checked - default => 一開啟網頁就選取
11. checkbox 
    * input type='checkbox' 可複選
    * checked - default => 一開啟網頁就選取
12. div
    * 用div將上面的h1, p, img, a, form ,ul等等包起來
13. html => head + body
    * head中含title
    * body為網頁顯示內容

### 最早接觸到的CSS
* [學習CSS版面配置](http://zh-tw.learnlayout.com/) - 點擊此標題可連結
    1. display
        * block - div就是一個區塊元素，會讓內容從新的一行開始並盡可能地充滿容器
        * inline - span就是行內元素，a也是行內元素
        * none - 會隱藏但不會刪除該原本內容的空間
        * inline-block - 建立一堆區塊自動填滿瀏覽器  
        ![](https://i.imgur.com/XvOkZrb.png)

    2. margin: auto;  
        * 區塊元素的width，避免左到右完全撐滿容器，可以設定左右外邊距(margin-left, margin-right)為auto  
        * margin: 0 auto; //第一個參數代表上下，第二個為左右
        * max-width -水平auto可能因為視窗比元素寬度還小導致出現水平捲軸，此時可以用max-width來解決

    3. Box Model  
        ![](https://i.imgur.com/0Y9OvCV.png)
        * 當設定元素的的寬度,但顯示卻超過你的設定=> 因為margin(邊框),paddgin（內距),border(邊界)都會撐開進而影響元素大小
        * 可以用box-sizing:border-box;來修飾 =>這個元素的margin,padding,border則都不會增加元素本身大小
        * -webkit-box-sizing => 支援safari
        * -moz-box-sizing => 支援firefox

    4. position
        * relative - 不會有特別表現，除非增加了額外的屬性  
        sample
            * top: -20px;
            * left: 20px;
            * width: 500px;
        ![](https://i.imgur.com/m2pKjSt.png)
        * fixed - 和relative一樣，會用top, right, bottom, left來進行定位。但是是相對於瀏覽器的視窗，就算頁面捲動還是固定在相同位置
        * absolute - 與fixed很像，但不一樣的地方是定位在該元素所處的上一層容器的相對位置
            * sample
            ![](https://i.imgur.com/afVSRBJ.png)
            * 如果上層容器沒有可以被定位的容器，就會依照該網頁的所有內容(body)進行定位，就會跟fixed相同
    
    5. float - 用在文繞圖
        * sample  
        ![](https://i.imgur.com/rxyZhMf.png)
        * clear屬性
            * 因為float是浮動的概念，很可能會漂浮在下一個元素上，導致看起來像是下一個元素將上一個元素包起來  
            ![](https://i.imgur.com/5jM2642.png)
            * 如果想要讓此狀況顯示順序正常則需要用到clear屬性，也可以直些使用clear:both;
            ![](https://i.imgur.com/jkQxZ5J.png)
    
    6. media queries - 響應式設計(Responsive Design)
        * 針對不同瀏覽器與裝置響應不同顯示效果  
        ![](https://i.imgur.com/BXz0KrK.png)

* freeCodeCamp
    1. 文字顏色, 字體大小
        * inline  
        <標籤 style="color: blue;">CatPhotoApp</標籤>
        * style -寫在html中
    2. 圖片大小 - width: 可以用px, %等等
    3. border
        * 顏色，寬度，實現虛線等
        * radius - 倒角，50%即是圓形
    4. background - 背景顏色，大小
    5. 透過id & class進行選擇
        * id = #
        * class = .
    6. padding - 調整內距
    7. margin - 調整邊框
    8. 選擇type
        * 選擇type是radio的 => [type='radio']
    9. 權重
        * 後面的CSS會覆蓋前面的
        * !important > inline的CSS > id > class > element
