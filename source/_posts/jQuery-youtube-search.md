---
title: "[jQuery] - 練習-Youtube Search Engine"
date: 2019-03-16 14:18:43
tags:
  - jQuery
category:
  - project
---

## Demo

jQuery 練習-Youtube Search Engine- [Demo](https://orow.github.io/MyProjects/ProjectsInJS&jQuery/YoutubeSearchEngine/index.html)

## 1. Introduction 介紹

利用 youtube 提供的 api 來達到搜尋後列出 list 的功能。
此功能是從 Udemy 的課程學習，現階段不是全部能了解，但還是先記錄下來，以後能夠 review 與修正。

頁面上在還沒搜尋前只有 header 與 search bar 加上 footer 的區塊，search bary 在 focus 與 blur（離開焦點）會有拉長與縮短的效果。
輸入關鍵字搜尋後會多出結果顯示：

- 搜尋結果清單：每頁五筆資料、左邊是縮圖、右邊是名稱，名稱下則包含上傳者、日期、敘述等。
- 上下頁按鈕：第一頁只會有 Next Page button。

![](https://i.imgur.com/9EWfqJ8.png)

引入 fancybox 並透過點擊搜尋結果的項目名稱來彈出視窗，顯示在網頁最上層。
![](https://i.imgur.com/7pVD3xo.png)

## 2. 功能

由於這個練習對我來說比較複雜，就拆成幾個部分來分別紀錄

- Search Bar 效果
- 列出搜尋結果清單
- Prev & Next Page 按鈕
- Fancybox 效果

全部有search()、nextPage()、prevPage()、getOutput()、getButtons()五個function。
前三個函式search、nextPage、prevPage中，都會用到getOutput產生網頁元素並append回網頁、getButtons產生按鈕並append回網頁。

### 2.1 Search Bar 效果

利用 jQuery 在 focus 與 blur（離開焦點）的時候執行 animate 來達到拉長與縮短的效果。
search bar 是由一個輸入區塊跟搜尋按鈕兩部分組成，輸入區塊的位置用`width`百分比控制、按鈕則是用`position: absolute`加上`right`的 px 來控制。
原始狀態，也就是 blur 時輸入區塊是用`width:45％`、按鈕是用`position: absolute`、`right:360px`。
點擊後變為 focus 狀態，輸入區塊則用`width:100％`、按鈕是用`right:10px`。

- focus
  ![](https://i.imgur.com/gKWiuWh.png)

- blur
  ![](https://i.imgur.com/W2iZl3a.png)

需要注意搜尋按鈕是用 submit，要用`.preventDefault()`來停止預設行為的發生。

### 2.2 列出搜尋結果清單

在網頁上輸入關鍵字後按下搜尋按鈕會直接得到 function search()的結果，用了`onsubmit="return search()"`的方式。

#### 2.2.1 發出 request
要發出 request 當然就是要用到 ajax 了，這邊用 get 方式來發出 request，要使用前需要先申請 youtube 的 API，這邊我也有做一些記錄：[Youtube Data API - Search Engine](https://orow.github.io/2019/03/17/Youtube-Data-API/)。

案例如下：

```js
$.get("url",
    {
      part: "snippet, id",
      q: q, //輸入的關鍵字的值
      type: "video",
      key: "Your_API_key"
    },function(){...}
)
```

- part: 每個搜尋的結果會帶有什麼內容：snippet 帶有搜尋結果的資訊，頻道 title、id、描述、縮圖、時間等，id：則為影片搜尋結果本身的 id。
- q: 指操作者輸入的關鍵字區塊的值。
- type: 可以選在播放清單或是影片等類型。

官方文件：[Youtube Data API - Search List](https://developers.google.com/youtube/v3/docs/search/list)
查詢各種 request 的內容，也可以直接在上面設定 request 的項目來測試會收到什麼。

Resonpse 則為
![](https://i.imgur.com/RPprBDj.png)

官方文件：Response中的items(search Resource)
[Youtube Data API - Search Resource](https://developers.google.com/youtube/v3/docs/search#resource)

part 設定的內容則是 resonpse 中的 items 裡面的陣列（search Resource）。
預設搜尋結果數量為 5 筆，也可以透過`maxResults`發出 request 修改。
![](https://i.imgur.com/6zj72YX.png)

程式碼中也有註解，有點雜，但是可以搭配程式碼一起看，個人覺得比較清楚。

```html
<form id="search-form" name="search-form" onsubmit="return search()">
  <div class="fieldcontainer">
    <input
      type="search"
      id="query"
      class="search-field"
      placeholder="Search YouTube..."
    />
    <input type="submit" name="search-btn" id="search-btn" value="" />
  </div>
</form>
<!-- Videos list -->
<ul id="results"></ul>
<!-- Prev & Next buttons -->
<div id="buttons"></div>
```

```js
// Search Function
function search() {
  // clear results
  $("#results").html("");
  $("#buttons").html("");

  // Get Form Input
  q = $("#query").val();

  // Run Get Rquest on API
  $.get(
    "https://www.googleapis.com/youtube/v3/search",
    {
      part: "snippet, id",
      q: q, // 輸入關鍵字區塊的值
      type: "video",
      key: "Your_API_key"
    },
    function(data) {
      var nextPageToken = data.nextPageToken;
      var prevPageToken = data.prevPageToken;
      // Log data - 資料很多，可以印出來回傳的資料再來比對
      console.log(data);

      // 回傳資料中的items是搜尋的每一個結果的詳細資料的陣列
      $.each(data.items, function(i, item) {
        // Get Output - getOutput是把資料加上html各種標籤跟內容的function
        var output = getOutput(item);

        // Display results
        $("#results").append(output);
      });

      // Button - getButtons是產生上一頁與下一頁按鈕的function
      var buttons = getButtons(prevPageToken, nextPageToken);
      // Display Buttons
      $("#buttons").append(buttons);
    }
  );
}
```
#### 2.2.2 回傳搜尋結果到網頁

在search function中把回傳資料的items中each出來，再帶回到getOutput的function中產生html標籤，最後append回網頁。
在組字串前建議用console.log印出結果來參照名稱，getOutput(item)中的item已經是each出來的每一筆資料，要的敘述就是id與snippet中的資料。
![](https://i.imgur.com/4WTZH8G.png)



```js
// Build Output
function getOutput(item) {
// infos from data
var videoId = item.id.videoId;
var title = item.snippet.title;
var description = item.snippet.description;
var thumb = item.snippet.thumbnails.high.url;
var channelTitle = item.snippet.channelTitle;
var videoDate = item.snippet.publishedAt;

// Build Output String
var output = '<li>' +
    '<div class="list-left">' +
    '<img src=" ' + thumb + '">' +
    '</div>' +
    '<div class="list-right">' +
    '<h3><a data-fancybox data-type="iframe" data-src="https://www.youtube.com/embed/' +videoId+ '"href="javascript:;"> '+ title + '</a></h3>' +
    '<small>By <span class="cTitle">' + channelTitle + '</span> on ' + videoDate + '</small>' +
    '<p>' + description + '<p>' +
    '</div>' +
    '</li>' +
    '<div classs="clearfix"></div>' +
    '';

return output;
}
```

### 2.3 Prev & Next Page 按鈕


在 serach 的 function 中會執行 getButtons 的 function 來產生按鈕，把產生後的按鈕 append 回到網頁中，兩個按鈕也都會帶有 onclick 的屬性來執行nextPage與prevPage的 function。

#### 2.3.1 nextPage & prevPage function
跟search的差別是在送出get request的時候多了一個`pageToken`，目的是要帶回回傳資料中nextPageToken或prevPageToken的字串，這裡也透過自訂屬性`data-token`來獲取字串。q則一樣是輸入關鍵字的值，也會帶回自訂屬性`data-query`中。

用 Next Page 的 function 來説明

```js
// NextPage function
function nextPage() {
  var token = $("#next-button").data("token");
  var q = $("#next-button").data("query");

  // clear results
  $("#results").html("");
  $("#buttons").html("");

  // Get Form Input
  q = $("#query").val();

  // Run Get Rquest on API
  $.get(
    "https://www.googleapis.com/youtube/v3/search",
    {
      part: "snippet, id",
      q: q, // 輸入關鍵字區塊的值
      pageToken:  token, // 需要是回傳的nextPageToken或prevPageToken字串
      type: "video",
      key: "Your_API_key"
    },
    function(data) {
      var nextPageToken = data.nextPageToken;
      var prevPageToken = data.prevPageToken;
      // Log data
      console.log(data);

      // 回傳資料中的items是搜尋的每一個結果的詳細資料的陣列
      $.each(data.items, function(i, item) {
        // Get Output - getOutput是把資料加上html各種標籤跟內容的function
        var output = getOutput(item);

        // Display results
        $("#results").append(output);
      });

      // Button - getButtons是產生上一頁與下一頁按鈕的function
      var buttons = getButtons(prevPageToken, nextPageToken);
      // Display Buttons
      $("#buttons").append(buttons);
    }
  );
}
```

#### 2.3.2 getButtons function

getButtons()中的參數是發出request後會回傳的資料，在程式中使用getButtons會先宣告變數如下：

```js
var nextPageToken = data.nextPageToken;
var prevPageToken = data.prevPageToken;
```

程式中有包含上一頁跟下一頁的參數，如果是第一頁的搜尋結果，youtube server不會傳送prevPageToken的字串回來，透過if else來判斷只需要下一頁的按鈕或者是兩者都需要。這邊會加上自訂屬性`data-token`、`data-query`等。

```js
// Build the Buttons
function getButtons(prevPageToken, nextPageToken){
  if (!prevPageToken){
      var btnoutput = '<div class="button-container">' + '<button id="next-button" class="paging-button" data-token="' +nextPageToken+'" data-query="'+q+'" onclick="nextPage();">Next Page</button></div>'; 
  } else{
      var btnoutput = '<div class="button-container">' + '<button id="prev-button" class="paging-button" data-token="' +prevPageToken+'" data-query="'+q+'" onclick="prevPage();">Prev Page</button>' + 
      '<button id="next-button" class="paging-button" data-token="' +nextPageToken+'" data-query="'+q+'" onclick="nextPage();">Next Page</button></div>'; 
  }
  return btnoutput;
}
```


## 3. Fancybox 效果

每一筆的搜尋結果的會使用上fancybox來展示影片與iframe遷入外部網頁的效果
[fancybox CDN](https://cdnjs.com/libraries/fancybox)
[fancybox文件](http://fancyapps.com/fancybox/3/docs/#iframe)

參照文件中iframe用法如下：
src的url使用youtube的遷入影片方式`https://www.youtube.com/embed/`加上從server回傳的`videoId`
[Youtube嵌入影片說明](https://support.google.com/youtube/answer/171780?hl=zh-Hant)

```html
<a data-fancybox data-type="iframe" data-src="https://www.youtube.com/embed/' +videoId+ '" href="javascript:;">
	Webpage
</a>
```

JavaScript: fancybox

```js
$('[data-fancybox]').fancybox({
  toolbar  : false,
  smallBtn : true,
  iframe : {
    preload : false
  }
})
```