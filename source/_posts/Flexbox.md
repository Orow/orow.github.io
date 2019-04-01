---
title: "[CSS] - Flexbox"
date: 2019-01-30 20:00:00
tags:
  - html
  - css
---

## Flexbox 模型概念

根據 W3C 的文章所介紹，flex 的盒子模型如下圖。
首先需在最外層用了 flex container 包覆所有的 item，就是要讓整個區塊都能夠具有 flex 的效果。
圖中提到的兩部分：main 就是水平 horizontally，cross 就是垂直 vertically。
從圖中也可以看到水平的部分：main start, main end。垂直的部分：cross start, cross end。
![](https://i.imgur.com/E11WsHp.png)

## Flexbox 屬性介紹

- display
- flex-direction
- justify-content
- align-items
- align-self
- align-content
- flex-wrap
- order
- flex

### display

CSS3 中多了`flex`的屬性，與 block 相像，都會換行，但多了更多彈性可以利用其他屬性來進行排版。
另外還有`inline-flex`的設定值，也是與 inline-block 用法一樣。
參考下面網路資源
![](https://i.imgur.com/v4rKyUB.png)

### flex-direction

顧名思義，這個設定值可以決定排列方向，垂直或水平。
下圖從左到右分別為：
`row`：由左到右，再從上到下(default)
`row-reverse`：與 row 相反
`column`：從上到下，再由左到右
`column-reverse`：與 column 相反
![](https://i.imgur.com/7NBehvE.png)

### justify-content

這個設定值則是決定了水平(horizontally)對齊的位置。
共有五種設定值：
`flex-start`：靠左對齊(default)
`flex-end`：靠右對齊
`center`：水平置中
`space-between`：平均分配，最左與最右會貼齊邊緣
`space-around`：平均分配，間距也是平均分配
![](https://i.imgur.com/FeNqYTt.png)

### align-items

align-items 與上面的 justify-content 正好相反，是來設定垂直(vertically)對齊的位置。
共有五種設定值：
`flex-start`:對齊上方邊緣
`flex-end`：對齊下方邊緣
`center`：垂直置中
`stretch`：內容撐開充滿容器(default)
`baseline`：把所有內容元素的基線當作對齊的標準
![](https://i.imgur.com/qGAul2R.png)

### align-self

設定值與 align-item 相同都有五種，但是是對個別 item 來進行修飾。
這邊用中間綠色的 item2 改為`baseline`在不同 align-items 設定值下的結果。
![](https://i.imgur.com/3mYb3I4.png)

### align-content

遇到多行的元素就需要使用到 align-content 這個屬性。
共有六種設定值：
`flex-start`：對齊上方邊緣(default)
`flex-end`：對齊下方邊緣
`center`：垂直置中
`space-between`：平均分配，最上方與最下方會貼齊邊緣
`space-around`：平均分配，間距也是平均分配
`strech`：內容元素全部撐開(default)
![](https://i.imgur.com/aW6bdrE.png)

### flex-wrap

這個是用來讓內容元素進行換行，當設定 display 為`flex`或是`inline-flex`的時候，就可以用 flex-wrap 來進行換行。
共有三種設定值：
`nowrap`：單行，超出的話會有 scroll bar(default)
`wrap`：多行
`wrap-reverse`：多行，但是會反轉
![](https://i.imgur.com/P49OjtB.png)

### order

order 這個屬性直接可以指定一個數值來進行從小到大的排列順序。
下面例子區塊中的數字為原本 html 區塊的順序，透過 order 排序後如下
（[範例轉載：css-flexbox-demo8.html](https://www.oxxostudio.tw/demo/201501/css-flexbox-demo8.html)）

```css
.order1 {
  order: 1;
  background: #c00;
}
.order2 {
  order: 2;
  background: #069;
}
.order3 {
  order: 3;
  background: #095;
}
.order4 {
  order: 4;
  background: #f50;
}
.order5 {
  order: 5;
  background: #777;
}
.order6 {
  order: 6;
  background: #077;
}
```

![](https://i.imgur.com/QsPw9GS.png)

### flex

此項目個人還尚未完全了解，先附上網路上找到的資源與案例。
flex 由三個屬性組合而成，依照先後順序分別是`flex-grow`,`flex-shrink`和`flex-basis`，如果 flex 只填了一個數值 (無單位)，那麼預設就是以 flex-grow 的方式呈現，至於三個屬性的解釋如下：

`flex-grow`：數字，無單位，當子元素的 flex-basis 長度「小」於它自己在父元素分配到的長度，按照數字做相對應的「伸展」比例分配，預設值為 0，不會進行彈性變化，不可為負值，設為 1 則會進行彈性變化。

`flex-shrink`：數字，無單位，當子元素的 flex-basis 長度「大」於它自己在父元素分配到的長度，按照數字做相對應的「壓縮」比例分配，預設值為 1，設為 0 的話不會進行彈性變化，不可為負值。

`flex-basis`：子元素的基本大小，作為父元素的大小比較基準，預設值為 0，也因為預設值為 0，所以沒有設定此屬性的時候，會以直接採用 flex-grow 屬性，flex-basis 也可以設為 auto，如果設為 auto，就表示子元素以自己的基本大小為單位。

三個屬性可以分開設定，也可以合在一起用一個 flex 統一設定，下面的例子展現出同一個 Flexbox，在不同的寬度，子元素會有不同大小的呈現。

```css
.flex {
  display: inline-flex;
  height: 60px;
  margin: 5px 5px 40px;
  border: 1px solid #000;
  vertical-align: top;
}
.flex-300 {
  width: 300px;
}
.flex-150 {
  width: 80px;
}
.item {
  height: 60px;
  text-align: center;
  line-height: 50px;
}
.item1 {
  flex: 1 2 200px;
  background: #c00;
}
.item2 {
  flex: 2 1 100px;
  background: #069;
}
```

![](https://i.imgur.com/eG8Xg6N.png)

範例中可以透過縮小放大視窗來看出區塊變化。
( 範例：[css-flexbox-demo9.html](https://www.oxxostudio.tw/demo/201501/css-flexbox-demo9.html))

### 參考資料
- [W3C](https://www.w3.org/TR/css-flexbox-1/#box-model)
- [OxxoStudio](https://www.oxxostudio.tw/articles/201501/css-flexbox.html)

### Flex練習 - 互動遊戲
- [Flexbox Froggy](https://flexboxfroggy.com/#zh-tw)