---
title: Youtube Data API - Search Engine
date: 2019-03-17 14:18:43
tags:
  - Youtube Data API
category: 
  - API
---

## 1. Create API key
### 1.1 Google Cloud Platform
ref : https://console.cloud.google.com/?hl=zh-TW
1. 確認有沒有專案repository
    * 如果沒有要新建立一個專案(it shoulde be one repository at least)
    ![](https://i.imgur.com/C1uYFCf.png)

2. 取用API & Services
    * 選單中選取`API和服務(API & Services)`
    * 選擇資訊主頁(Dashboard) => `啟用API和服務(Enable APIS & SERVICES)`
    ![](https://i.imgur.com/74Uj0fc.png)

    * 選擇Youtube Data API v3 and 啟用(Enable)
    ![](https://i.imgur.com/jNhGNEi.png)

3. 新增憑證
    * 啟用後進入到Youtube Data API v3的管理(Manage)頁面並選取憑證(credentials)
    ![](https://i.imgur.com/QFkkGXe.png)

    * API管理員中的「憑證」頁面(Credentials in the API Manager)
    * 建立憑證(Create credentials) => API key
    ![](https://i.imgur.com/Jy4dXS1.png)


## 2. How to use - Search Engine

此部分對我現階段來說還是比較複雜，沒有理解到很多，主要是透過網路上課程來進行這部分的運用（使用jQuery）。
**如果有錯請不吝嗇的告知小弟**
下面是這次的例子:

### 2.1 Example: Youtube Search Engine

Link: [Youtube Search Engine](https://orow.github.io/2019/03/16/jQuery-youtube-search/)


### 2.2 HTTP request

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



### 2.3 Server Response

Resonpse 則為
![](https://i.imgur.com/RPprBDj.png)

Response中的items(search Resource)
![](https://i.imgur.com/dGhJw1i.png)


官方文件：[Youtube Data API - Search Resource](https://developers.google.com/youtube/v3/docs/search#resource)


### 2.4 prevPage & nextPage

可以在$.get裡面的物件加上`{pageToken: token}`

```js
$.get(
  "https://www.googleapis.com/youtube/v3/search",
  {
    part: "snippet, id",
    q: q, // 輸入關鍵字區塊的值
    pageToken:  token, // 需要是回傳的nextPageToken或prevPageToken字串
    type: "video",
    key: "Your_API_key"
  },
)
```

這邊的token是要帶回回傳資料中nextPageToken或prevPageToken的字串。
發出的request中要寫入pageToken的request，在這個callback function中就可以利用`prevPageToken`跟`nextPageToken`的字串進行上下頁的切換。

### 2.5 Video information

發出的request中有`part: 'snippet, id'`。
可以透過search Resource中的snippet獲得影片的發佈時間、title、description、縮圖連結等。
也可以透過id來獲取種類、頻道、播放清單等的資訊。
![](https://i.imgur.com/4WTZH8G.png)

## 3. 參考資料
1. 官方資料
    [Getting-started](https://developers.google.com/youtube/v3/getting-started)
    [Search Lists](https://developers.google.com/youtube/v3/docs/search/list)
    [Search resources](https://developers.google.com/youtube/v3/docs/search#resource)
2. 網路資料
    Request中除了part參數跟其他的部分: https://ithelp.ithome.com.tw/articles/10194007