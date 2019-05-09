---
title: "[JavaScript] - Localstorage"
date: 2019-03-10 23:00:00
tags:
  - javascript
categories:
  - javascript
---
本篇主要是透過線上課程：`udemy`、`w3school`及網路上搜尋資源所學習的。

## localstorage

`localstorage`是瀏覽器裡面的資料庫，比如說瀏覽商城的時候，會有欄位顯示瀏覽過的商品，這就是透過 localstorage 的方式紀錄下來的。要注意到，localstorgae 儲存資料的方式是以`string`的方式，也就是`字串`。
在 chrome 瀏覽器可以透過`application`中去查詢`localstorage`
下面會介紹幾個操控 localstorage 的方式。

### setItem & getItem

 等同字面上的意思，設定項目

```js
var str = "John";
localstorage.setItem("myName", str);

console.log(localstorage.getItem("myName"));
// John
```

透過 chrome 瀏覽器觀察
![](https://i.imgur.com/KTNnIlh.png)

進一步設計點擊後儲存資料，與點擊後出叫名字的方式。

```html
<h2>請輸入你的姓名</h2>
<input type="text" class="textClass" />
<input type="button" class="btnClass" value="點擊" />
<input type="button" class="btnCall" value="點擊呼叫名字" />
```

```js
var btn = document.querySelector(".btnClass");
function saveName(e) {
  var str = document.querySelector(".textClass").value;
  localStorage.setItem("myName", str);
}
btn.addEventListener("click", saveName);

var call = document.querySelector(".btnCall");
function callName(e) {
  var str = localStorage.getItem("myName");
  alert(str);
}
call.addEventListener("click", callName);
```

透過 chrome 瀏覽器觀察
![](https://i.imgur.com/M2qioTs.png)

### 資料轉換

前面提到`localstorage`只會保存`string`的資料，所以這邊要學習字串與陣列的資料轉換。

- `JSON.stringify()`: 轉為 JSON 格式的字串
- `JSON.parse()`: 從 JSON 格式的字串轉為原本的資料內容與型別

會用在要儲存一個陣列裡面有多筆資料的時候，(JSON 格式)。
下面例子則為將陣列中的農夫名稱轉為字串後，儲存進去 localstorgae，再把字串轉回原本型別後取出。

```js
var county = [{ farmer: "約翰" }];

var countyString = JSON.stringify(county);
console.log(countyString);
localStorage.setItem("countyItem", countyString);

var getData = localStorage.getItem("countyItem");
console.log(getData);

var getDataArray = JSON.parse(getData);
console.log(getDataArray[0].farmer);
```

### dataset - 抓取自訂資料

在 html 中，某些情況下會有自訂的屬性，以下面的例子來說
`data-num="1"`, `data-dog="3"`就是自訂的屬性。

```html
<ul class="list">
  <li data-num="1" data-dog="3" class="listli">約翰</li>
</ul>
```

自訂的屬性還是可以透過 javascript 來獲取資訊。

```js
// ----data-set 抓取資料----
var dataSet = document.querySelector(".listli").dataset;
console.log(dataSet); // 會列出所有的"data-*"屬性的所有資訊:{num: "1", dog: "3"}

var dataSetDetail1 = document.querySelector(".listli").dataset.num;
console.log(dataSetDetail1); // 取得 data-num的資訊: "1"

var dataSetDetail2 = document.querySelector(".listli").dataset.dog;
console.log(dataSetDetail2); // 取得 data-dog的資訊: "3"

var listLi = document.querySelector(".listli");
// 利用target來確認點擊的人名以及相關資訊

function checkList(e) {
  var num = e.target.dataset.num;
  var dog = e.target.dataset.dog;
  console.log("第" + num + "個人");
  console.log("有" + dog + "隻狗");
  // 第1個人
  // 有3隻狗
}

listLi.addEventListener("click", checkList, false);
```

### dataset & array 運用

原本的 html 沒有自訂屬性，但想要透過 js 動態加入。
下面的例子是，點擊哪個農夫後，會 alert 是選擇到哪一個農夫。

```html
<ul class="list"></ul>
```

要注意到用 innerHTML 新增 html 元素或是 splice 去刪除後，都需要去執行資料更新。

```js
var county = [
  {
    farmer: "卡斯伯"
  },
  {
    farmer: "查理"
  },
  {
    farmer: "哈維"
  }
];
var list = document.querySelector(".list");

//更新農場資料
function updateList() {
  var str = "";
  var len = county.length;
  for (var i = 0; len > i; i++) {
    str += "<li data-num=" + i + ">" + county[i].farmer + "</li>";
  }
  list.innerHTML = str;
}
updateList();

//確認點擊的農夫是誰
function checkList(e) {
  var num = e.target.dataset.num;
  if (e.target.nodeName !== "LI") {
    // nodeName是大寫的節點名稱，這邊點擊到的是li
    return;
  }
  county.splice(num, 1);
  updateList();
}

list.addEventListener("click", checkList, false);
```
