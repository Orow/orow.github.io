---
title: Youtube Data API - Search Engine
date: 2019-03-17 14:18:43
tags:
  - Youtube Data API
category: 
- API
toc: true
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
### 2.1 Project Link: Youtube Search Engine
Seacrch Result code: 

```js
$.get(
      "https://www.googleapis.com/youtube/v3/search", {
          part: 'snippet, id',
          q: 'keywords which you type in',
          type: 'video',
          key: 'YOUR_API_KEY'
      },
      function (data) {
          var nextPageToken = data.nextPageToken;
          var prevPageToken = data.prevPageToken;
          // Log data
          console.log(data);
          $.each(data.items, function (i, item) {
              // Get Output
              var output = getOutput(item);

              // Display results
              $('#results').append(output);
          });
          // Button
          var buttons = getButtons(prevPageToken, nextPageToken);
          // Display Buttons
          $('#buttons').append(buttons);
      }
  );
}
```

### 2.2 HTTP request
get的url需要為https://www.googleapis.com/youtube/v3/search。
part的部分是針對使用者需要用到的部分去發出request。
### 2.3 function中data的sample(search Resource)

  ```js
  {
    "kind": "youtube#searchListResponse",
    "etag": etag,
    "nextPageToken": string,
    "prevPageToken": string,
    "regionCode": string,
    "pageInfo": {
      "totalResults": integer,
      "resultsPerPage": integer
    },
    "items": [
      search Resource
    ]
  }

  // 以下為上面 "items": [
  //    search Resource
  //  ]裡面的search resource
  {
    "kind": "youtube#searchResult",
    "etag": etag,
    "id": {
      "kind": string,
      "videoId": string,
      "channelId": string,
      "playlistId": string
    },
    "snippet": {
      "publishedAt": datetime,
      "channelId": string,
      "title": string,
      "description": string,
      "thumbnails": {
        (key): {
          "url": string,
          "width": unsigned integer,
          "height": unsigned integer
        }
      },
      "channelTitle": string,
      "liveBroadcastContent": string
    }
  }

  ```

### 2.4 prevPage & nextPage
可以在$.get裡面的物件加上`{pageToken: token}`
就可以在search Resource中去取用`prevPageToken`跟`nextPageToken`。

### 2.5 Video information
發出的request中有`part: 'snippet, id'`。
可以透過search Resource中的snippet獲得影片的發佈時間、title、description、縮圖連結等。
也可以透過id來獲取種類、頻道、播放清單等的資訊。

## 3. 參考資料
1. 官方資料
    [Getting-started](https://developers.google.com/youtube/v3/getting-started)
    [Search resources](https://developers.google.com/youtube/v3/docs/search#resource)
2. 網路資料
    Request中除了part參數跟其他的部分: https://ithelp.ithome.com.tw/articles/10194007