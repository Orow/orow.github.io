---
title: 切版練習-LoopLAB - Bootstrap
date: 2019-02-13 14:18:43
tags: 
    - bootstrap
category: test
---

# Note - LoopLAB - from 2019/02/13(one day)

###### tags: `Udemy` `bootstrap`
#### Bootstrap練習-Project1

## Navbar
1. Navbar背景為淺灰（bg-dark),搭配文字顯示（navbar-dark)
![](https://i.imgur.com/0Yxddwr.png)
2. 此處要border-bottom
## Home-Section
1. 包法
    =>home-section
    =>overlay(背景圖覆蓋透明黑)
    =>container
    =>row
    =>col-8;col-4
    a. col-8的部分(用display-4)
    1. 分為h1,跟3個div
    2. 每個div都用d-flex,因為每個div中左邊有font-awesome,右邊有文字
    3. 左(align-self-start);右(align-self-end)
![](https://i.imgur.com/HJwDCOD.png)

    b. col-4的部分
    1. 用card(bg-primary)包=>card-body
    2. h3,p,form
    3. form中4個form-group
    4. 最後一個不用form-group只用input（因為form-group有mb)
    5. input用submit(class=btn,btn-block,btn-outline-light)
=>*這裡用light是因為card本身選了bg-primary
![](https://i.imgur.com/MCuXmUh.png)

2. 背景圖片與overlay
    a. 背景圖片：
    background: no-repeat, center/cover, fixed(這個是會固定圖片不會隨滑鼠scrolling)
    b. overlay:
    position:absolute;給width:100%,再給高度與透明度

## Explore head section
1. 包法
    =>section
    =>container
    =>row
    =>col,text-center,py-5
    =>h1(display-4),p(lead),a(btn,btn-outline-secondary)

## Explore section
1. 包法
    =>section(bg-light,text-muted,py-5)
    =>container
    =>row
    =>col-6,col-6
    a. 左邊col-6
    1. 背景圖（img-fulid,rounded-circle)
    b. 右邊col-6
    1. h3,p,div*2
    2. div(d-flex),裡面跟上面home-section差不多;每個div中左邊有font-awesome,右邊有文字=>左(align-self-start);右(align-self-end)

2. 右邊font-awesome顏色跟底色不同記得要修改

## Create & Share section
都是從explore section複製過來再改顏色或換位置

## Footer
1. 包法
    =>footer(bg-dark)
    =>container
    =>row
    =>col,text-white.py-4
2. Contact Us button "Modal"
    =>這裡用modal(javascript)
    button class="btn btn-primary" data-toogle="modal" data-target="#contactModal"
    `data-target為後續要呼叫的ID名稱`

## Contact Us Modal
1. 包法
    =>div class="modal fade text-dark show"
    =>div class=modal-dialog
    =>div class=modal-content
    =>content中分三層
    ![](https://i.imgur.com/lnLaxIf.png)
    1. div class=modal-header
        a. h5 class=modal-title
        b. button class="close" data-dismiss="modal" 
        `data-dismiss會關閉這個的彈出視窗`
    2. div class=modal-body
        a. 用form包 =>form-group*3
        b. 前兩個form-group中包label & input
        c. 第三個form-group中用label & textarea
        d. label for , input type class="form-control, textarea class="form-control"
    3. div class=moadl-footer
        button class="btn btn-primary btn-block"
    ref:https://bootstrap.hexschool.com/docs/4.0/components/carousel/

---
## Question
1. Navbar的menu如何點選後字體顯示不同顏色？
2. submit要用button or input type=submit?
3. 這裏overlay用position:absolute他的父元素看例子沒有用position:relative;原因是什麼？