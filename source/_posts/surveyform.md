---
title: "[手刻] - 網頁練習-SurveyForm"
date: 2019-01-21 14:18:43
tags:
  - html
  - css
category:
  - project
---

這是剛開始透過freeCodeCamp學習html，css差不多過10天後第二個案例，透過修改一些內容來感受html與css的實際效果，是第一次修改網頁上看到的東西。

## Demo

網頁練習-Survey Form - [Demo](https://orow.github.io/MyProjects/SurveyFrom/index.html)

## 1. Introduction 網頁介紹

這個網頁也是從[Code Pen](https://codepen.io/freeCodeCamp/full/VPaoNP)上的例子修改過來的。
主要都是表單的部分，body下先一個h1標題後再用div包住一個p文字敘述加上整個form表單。
form表單每一列都用div包住，左邊是label，右邊有些是input type=text, email, number, radio, checkbox或是select加上option等，最後是submit的button。

## 2. Layout Notes

表單中的label都會用到for，這邊label用的for會跟input的name內容一樣，另外不同的input type會有不同的輸入格式，輸入錯誤的時候submit出去都會有提示。
如果在標籤中使用`required`代表submit的時候該欄位是一定要填的。
radio的部分如果只想要單選，記得每一項的name要輸入相同的值。
![](https://i.imgur.com/ovZXOyY.png)

