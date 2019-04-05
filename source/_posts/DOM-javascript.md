---
title: "[JavaScript] - DOM"
date: 2019-03-09 20:00:00
tags:
  - javascript
categories:
  - javascript
---

本篇主要是透過線上課程：`udemy`、`w3school`及網路上搜尋資源所學習的。

## DOM

DOM(document object modal)是 HTML、XML 和 SVG 文件的程式介面。它提供了一個文件（樹）的結構化表示法，並定義讓程式可以存取並改變文件架構、風格和內容的方法。DOM 提供了文件以擁有屬性與函式的節點與物件組成的結構化表示。節點也可以附加事件處理程序，一旦觸發事件就會執行處理程序。 本質上，它將網頁與腳本或程式語言連結在一起。
一個網頁是一個文件檔案，可以在瀏覽器或作為 html 的 code 顯示出來。DOM 可以用來儲存與操作進而修改該內容呈現。

- [參考來源 1](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)
- [參考來源 2](https://ithelp.ithome.com.tw/articles/10094965)

DOM 是由節點（node）組成的，如同下面範例，HTML 裡面每個元素、屬性都代表著其中一個節點

html code:
![](https://i.imgur.com/6BPKuuR.png)

DOM:
![](https://i.imgur.com/gwCGCt9.png)

## DOM 操作的基本語法

- 更多各種操作可以在[w3school](https://www.w3schools.com/js/js_htmldom.asp)查詢

### 選取元素

getElementById()
getElementBy系列的是動態的 node list，每次調用都會重新對 document 進行查詢。
這邊提到的 ID 基本上在網頁中會是獨一無二。

querySelector()
querySelector系列的是靜態的 node list，對 document 的操作不會產生影響。
可以以 CSS selector 方式來選擇元素，id 用`#`，class 用`.`。

```html
    <h1 id="titleId" class="titleClass"></h1>
```

以下兩個操作方式都是會選到 h1 這個元素。

```js
// getElementById
document.getElementById('titleId');

// querySelector
document.querySelector('.titleClass');
```

querySelector也可以用來選擇下一層的元素，下面例子就會選取到`em`這個元素。
 
```html
    <h1 id="titleId" class="titleClass"><em>123</em></h1>
```
```js
// querySelector
document.querySelector('.titleClass em');
```

### setAttribute
setAttribute("屬性名稱", "屬性內容")，可以透過這個方式來動態新增屬性然後帶入CSS條件進行修飾，或者修改`<a href="">`的連結內容。

```js
// 先選取navbar中的a連結
var el = document.querySelector('#navbar a');

// 在a中新增一個叫做strId的id名稱，css就可以先寫好對strId樣式設定
el.setAttribute("id","strId");

// 下面做法可以讓點擊ａ連結後改為跳轉到google網頁
el.setAttribute("href","https://www.google.com.tw/");
```

另外提到一個`getAttribute`，是用來取出選取的屬性名稱的內容


```js
var el = document.querySelector('#navbar a');
var hrefContent = el.getAttribute('href');
console.log(hrefContent);
// 以上面例子為例就會撈到 https://www.google.com.tw/ 這個網址
```


### innerHTML & textContent
- innerHTML
會取出包含標籤與內部的所有內容
- textContent
只取出純文字的內容，不會取出標籤

```html
<div id="main" class="main"><p>Hello World!</p></div>
```

```js
// innerHTML
var main = document.getElementById('main').innerHTML;
// 取出內容為<p>Hello World!</p>

//textContent
var text = document.querySelector('.class').textContent;
// 取出內容為Hello World!
```

需要注意的是使用innerHTML放入的內容會帶有HTML屬性作用，如果用`script`寫一段攻擊會是有作用的。

### createElement
從名字意思上來說就是新增元素，但是只有單純新增元素，如果需要修改內容或是設定屬性還需要用到上面提到的其他設定值，像是textContent、setAttribute。
例子：
新增`em`並新增內容`1234`，之後再新增子節點到h1之後。
appendChild的用法為新增子節點到選擇的元素之後。


```html
<h1 class="title">
    <em>title</em>
</h1>
```

```js
var str = document.createElement('em');
str.textContent = '1234';
// 增加子節點
document.querySelector('.title').appendChild(str);

```

網頁上所見如下圖
![](https://i.imgur.com/XDZG6qh.png)

**createElement並不會把原本的em所取代，而innerHTML會先將原有的內容全部刪除後再做新增**

比較用Javascript操控HTML的兩個方法
- innerHTML
    1. 方法：組完字串後,傳進語法進行渲染
    2. 優點：效能快
    3. 缺點：資安風險,要確保來源沒問題
- createElement
    1. 方法：以DOM節點來處理
    2. 優點：安全性高
    3. 缺點：效能差
