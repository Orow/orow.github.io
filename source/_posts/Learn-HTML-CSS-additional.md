---
title: Learn HTML & CSS(補充)
date: 2019-01-28 14:18:43
tags:
  - html
  - css
---

## HTML

### Table

網頁中也可以顯示表格
`<table>`代表表格
`<tr>`為 row 是橫列的意思。
`<th>`代表表格的標題，會有粗體`font-weight:bold`的效果
`<td>`為 data 是直行的意思，多少個`<td>`就有多少行。
框線則可以透過 CSS 中的 border 進行修飾，例子用上了`border: 1px solid #000;`
![](https://i.imgur.com/DKyx0wP.png)

如果要像一般表格不要有間隔，則在 CSS 對 table 加入以下兩個參數：
`border-collapse:collapse`，這會讓彼此之間框線可以合併。
`border-spacing:0;`讓距離變成 0px。
![](https://i.imgur.com/e17lGhu.png)

html code:

```html
<table>
  <thead>
    <tr>
      <th>#</th>
      <th>Title</th>
      <th>Category</th>
      <th>Date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>Post One</td>
      <td>Web Development</td>
      <td>Jan 28 2019</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Post Two</td>
      <td>Tech Gadgets</td>
      <td>Jan 28 2019</td>
    </tr>
  </tbody>
</table>
```

### Label 與 Input 搭配

在點擊 label 後可以讓游標跳到後面則欄位上直接輸入，提升使用者體驗
在`label for="mail"`for 中為 mail。
input 元素的 id 就取名相同的名稱為 mail，`input id="mail"`即可以達到該效果。
![](https://i.imgur.com/HkxixzR.png)

html code:

```html
<label for="mail">Email: </label>
<input type="text" id="mail" placeholder="Your mail" />
```

## CSS

### Pseudo Class & Element 偽類別與偽元素

偽類(pseudo class)：在選擇已經存在的東西，如 a:hover 是選擇`<a>`的某種狀態
偽元素(pseudo elemnet)：創造一個新的假元素，沒有被任何 tag 包住的內容
單冒號 (:) 是用在偽類
雙冒號 (::) 則是用在偽元素

pseudo-class 的
在某些狀態被選取的話，會被加進 selector 中，如`:hover`是滑鼠停在某個元素時就會套用指定選擇器的效果，`a:hover`default 的效果就是有下底線。
![](https://i.imgur.com/xZbWreq.png)

pseudo-element
::first-line：`div::first-line {color:red;}`選取第一行，在沒有任何 tag 包住的情況下創造新的假元素
下面例子就會產生第一行為紅色的結果
![](https://i.imgur.com/stk8aMe.png)

```html
<div>
  Hello Everyone!! My Name is Candy
</div>
```

參考資料

- [MDN web docs](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes)
- [【CSS FAQ】偽元素 (PSEUDO ELEMENT) 和偽類 (PSEUDO CLASS) 差在哪？](https://stringpiggy.hpd.io/pseudo-element-pseudo-class-difference/)

### Background-image

透過 CSS 直接插入背景圖片，設定好寬度與高度後，系統預設是 x，y 方向都會 repeat。
![](https://i.imgur.com/bmaeJ4K.png)

```css
.box {
  background: url("../img/large-3-11.jpg");
  height: 1800px;
  width: 2700px;
}
```

透過在 background 後加上`no-repeat`, `repeat-x`, `repeat-y`可以自己控制背景圖。
`repeat-x`
![](https://i.imgur.com/kn6agpY.png)
`repeat-y`
![](https://i.imgur.com/8ga1jGq.png)
`no-repeat`,加上`center`即可置中
![](https://i.imgur.com/wfaaZIw.png)
如果加上`center/cover`，置中並延伸到所設定的寬度與高度充滿整個容器。
當背景圖片通常都會加上`center/cover`。
![](https://i.imgur.com/bz1ltpO.png)

### Em & Rem 相對數值單位

em 是相對的單位，文字大小是相對於上一層的父元素(parent element)。
rem 是相對的單位，文字大小是相對於最外圍的根元素(root element)，也就是`<html>`標籤。

em 因為是相對上一層父元素的倍數，如果包很多層會被影響。
ex.外層的文字是 1.4em，內層的 1em 文字大小等於外層 1.4em，隨後的逐漸放大。
如果再加入一層在內部，文字就會上一層的大小再乘上設定的 em 再放大。
![](https://i.imgur.com/tfXGdc1.png)
rem 只要不改到網站 root 層的文字大小，就可以清楚的計算需要大小多少的文字，是 root 的倍數。
ex.網站文字大小 16px，1rem ＝ 16px;最上跟最下面用 em 來比較
![](https://i.imgur.com/92MLHSS.png)

參考資料：

- https://www.hexschool.com/2016/01/02/2016-08-08-em-vs-rem/

### Vh & Vw 螢幕可視範圍的百分比

Vh 是螢幕可視範圍高度的百分比，Vw 是螢幕可視範圍寬度的百分比。
會隨著視窗大小來變動，適合用在landing page跟RWD的網頁。

一般設定
![](https://i.imgur.com/LStlb9s.png)

```css
header {
  background: #000;
  color: #fff;
  height: 100%;
  width: 100%;
}
```

Vh & Vw
將height改為`height:100vh`，`width:50vh`
![](https://i.imgur.com/ynwXXOt.png)


## 相容性確認
有些語法是在新的CSS3中才有，可以透過這個網站來確認語法在哪些瀏覽器可以使用。
[Can I Use](https://caniuse.com/)