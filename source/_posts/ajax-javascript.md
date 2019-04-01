---
title: AJAX - JavaScript
date: 2019-03-11 21:00:00
tags:
  - javascript
categories:
  - javascript
---

## AJAX

概論

AJAX 並非是一個技術（Technology），而是一種網站設計的架構（Architecture），雖然主要以 JavaScript 與 XML 為主，但還包括其它成員，也就是 CSS、DOM（Document Object Model）與 HTML 等，特別是 XMLHttpRequest 元件，使 AJAX 能達到非同步資料交換的目的。

`非同步資料交換（asynchronous）`： 瀏覽器不需重新整理就可以跟後端程式或是資料庫傳送接收資料

在 Jesse Garrett 的文章中，對 AJAX 的定義如下：
● 使用 XHTML 與 CSS 作為展現標準
● 使用 DOM 作為動態顯示與互動
● 使用 XML 與 XSLT 作為資料交換與運用
● 使用 XMLHttpRequest 作為非同步的資料回饋
● 使用 JavaScript 結合以上所有結果

[參考來源](https://www.ithome.com.tw/node/33060)

像是一般在註冊表單頁面，要輸入新增帳戶，有時候打完到一半或是打完帳號後，就有提示說已經有人註冊過。或是購物網站中的庫存數量透過選取數量或者是加入購物車後會顯示剩餘庫存，並提示是否能繼續下一步購買流程。
這些都是透過 ajax 的方式，不需要重新整理讀取整個頁面才能顯示。

### XMLHttpRequest

透過 XMLHttpRequest 來獲取瀏覽器資料。

首先要先產生`XMLHttpRequest()`才能跟其他伺服器要資料，執行這個 request 後，就會出現相對應資料。
Chrome 瀏覽器中 dev tool 中的 console：
![](https://i.imgur.com/pDb67jR.png)

其中 readyState 就是代表目前狀態

    readtState
    0 - (request not initialized)已經產生一個XMLHttpRequest，但還沒有連結要撈的資料。
    1 - (server connection established)用了open()，但還沒有把資料傳送過去。
    2 - (request received)偵側到用了send。
    3 - (processing request)資訊處理中。
    4 - (request finished and response is ready)撈到資料，數據完全接收到了。

要用`open()`指令來初始設定，
其中第一個參數是發出 request 的格式，第二個是讀取的網址，第三個是同步(false)與非同步(true) 。

格式則有`get`(讀取)，`post`(傳送資料到伺服器)。
如下面程式碼：

```js
var xhr = new XMLHttpRequest();
// 格式,要讀取的網址,同步與非同步
// 格式: get(讀取), post(傳送資料到伺服器)
xhr.open("get", "這裡放網址", true);
// 空值，單純geta，沒有要傳資料，括號中用null
xhr.send(null);
```

如果資料傳輸完畢且成功，在 XMLHttpRequest 中的 readyState 會等於 4，responseText 則是內容。

### Asynchronous(非同步)概念

在使用 ajax 的`open()`這個指令的時候會用非同步這個概念 。
在 open 裡面第三個欄位的參數為。

- true: 非同步,不會等資料傳回來就讓程式繼續往下跑,等到資料跑完才自動回傳。
- false: 同步,會等資料跑完回傳回來,程式再繼續往下跑。

有些伺服器的資料是很龐大的，一般來說不太可能等全部跑完再執行之後的程式碼，所以就會用非同步的概念。
但是非同步的狀況會遇到下面的狀況
程式碼中的 console.log 不會有東西出現，但之後去確認 xhr 變數卻是成功的。

```js
var xhr = new XMLHttpRequest();

xhr.open(
  "get",
  "https://data.kcg.gov.tw/dataset/a98754a3-3446-4c9a-abfc-58dc49f2158c/resource/48d4dfc4-a4b2-44a5-bdec-70f9558cd25d/download/yopendata1070622opendatajson-1070622.json",
  true
);

xhr.send(null);

console.log(xhr.responseText); // 沒有印出東西
```

這種情況就需要用到`onload`這個事件來解決。
意思就是當傳送後載入完成再去執行這個`onload`帶的 function。

舉一個非同步也能夠處理的例子

- url 這邊是高雄市 opendata-電動機車充電站的資訊
- 用 foor 迴圈把種類是公共充電站加上位置名稱後，新增到清單中

```html
<ul class="list"></div>
```

```js
var xhr = new XMLHttpRequest();

xhr.open(
  "get",
  "https://data.kcg.gov.tw/dataset/a98754a3-3446-4c9a-abfc-58dc49f2158c/resource/48d4dfc4-a4b2-44a5-bdec-70f9558cd25d/download/yopendata1070622opendatajson-1070622.json",
  true
);

xhr.send(null);

xhr.onload = function() {
  console.log(xhr.responseText);
  //轉換字串為原資料型別（這邊是JSON)
  var str = JSON.parse(xhr.responseText);
  for (var i = 0; i < str.length; i++) {
    var kind = str[i].Kind;
    if (kind == "公共充電站") {
      document.querySelector(".list").innerHTML +=
        "<li>" + str[i].Location + "</li>";
    }
  }
};
```

總結整個流程分為以下：

1. 建立 XMLHttpRequest
2. 傳送到對方伺服器並要資料
3. 回傳到自己的瀏覽器
4. 拿到資料後再看要怎麼處理

### HTTP 狀態碼

可以透過 chrome 瀏覽器開發工具中的 Network 去確認，選擇 JS 後看到有`Status`的部分
這邊是用 google mpas 去打開
![](https://i.imgur.com/WMkEuLP.png)

status = 200，資料有正確回傳，有撈到。
status = 404，資料讀取錯誤，沒有撈到。

也可以利用 if 判斷式(xhr.status == 200)，來確認有沒有撈到資料，條件成立再去執行要做的行為。

詳細 http 狀況碼可以參考 [MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

### CORS

CORS(Cross-Prigin Resource Sharing)就是跨網域分享。
這邊用行政院 opendata 中[紫外線](http://opendata.epa.gov.tw/webapi/Data/UV/?$orderby=PublishTime%20desc&$skip=0&$top=1000&format=json)的資料，直接在網路上打開 JSON 資料是可以瀏覽。
但用 ajax 方式去發出 request 會顯示下面狀況。

![](https://i.imgur.com/6LGJsug.png)

這是 CORS 沒有被開啟的原因，可能是怕資安或是其他因素問題而不打開。

紀錄一個網站可以偵測 url 有沒有辦法接受開放資料的存取。
[test-cors.org](http://www.test-cors.org/)

用行政院 opendata 的紫外線網址去發出 request 回得到 status:0
![](https://i.imgur.com/xXTCXLX.png)

### POST Method

`POST`可以把資料傳到遠端資料去執行行為。
POST 相比 GET 需要在 open 之後多一個指令`setRequestHeader`，告訴伺服器現在要傳送資料、並告知資料的格式。而`setRequestHeader`與`send`也會因為格式不同而要傳送不同的內容。

這邊用註冊的概念來舉例，送出要註冊的帳號密碼，可以透過 xhr 中的`responsText`來確認是不是有相同帳號。

傳統格式

```js
var xhr = new XMLHttpRequest();
xhr.open('post','url',true);
xhr.setRequestHeader(“Content-type”,“application/x-www-form-urlencoded”);
xhr.send("email=abcde1111@gmail.com&password=12345");
```

JSON 格式

```js
var account = {
  email: "qweasdqq@gamil.com",
  password: "555"
};
var xhr = new XMLHttpRequest();
xhr.open("post", "url", true);
xhr.setRequestHeader("Content-type", "application/json");

// send()中需要用字串傳進去
var data = JSON.stringify(account);
xhr.send(data);
```
