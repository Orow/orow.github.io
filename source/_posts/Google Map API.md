---
title: Google Map API使用
date: 2019-01-31 14:18:43
tags:
  - Google Map API
category: 
- API
toc: true
---

## 1. Create API key
### Google Cloud Platform
ref : https://console.cloud.google.com/?hl=zh-TW
1. 確認有沒有專案repository
    如果沒有要新建立一個專案(it shoulde be one repository at least)
    ![](https://i.imgur.com/C1uYFCf.png)

2. 取用API & Services
    * 選單中選取`API和服務(API & Services)`
    * 選擇資訊主頁(Dashboard) => `啟用API和服務(Enable APIS & SERVICES)`
    ![](https://i.imgur.com/74Uj0fc.png)

    * 選擇Maps Javascript API and 啟用(Enable)
    ![](https://i.imgur.com/dFsPaCl.png)

3. 新增憑證
    * 啟用後進入到Maps Javascript的管理(Manage)頁面並選取憑證(credentials)
    ![](https://i.imgur.com/GgMab9B.png)

    * API管理員中的「憑證」頁面(Credentials in the API Manager)
    * 建立憑證(Create credentials) => API key
    ![](https://i.imgur.com/Jy4dXS1.png)

## 2. How to use in Javascript
### 2.1 Google maps docs
ref : https://developers.google.com/maps/documentation/javascript/tutorial
1. API key要放在script src中的'YOUR_API_KEY'的位置。
script code 如下：

    ```js
    <script>
        var map;
        function initMap() {
            map = new google.maps.Map(document.getElementById('map'), {
            center: {lat: -34.397, lng: 150.644},
            zoom: 8
            });
        }
    </script>
    <script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY&callback=initMap"
        async defer>
    </script>
    ```
    上面的document.getElementById('map')，代表在html中需要有個div class="map"。

2. Latitude & Longitude 緯度與經度取得
    * 可以從網站上獲得
        => https://www.latlong.net/
    * google map上點擊位置後,從網址中可以撈到
    
## 常見應用

  
<a href="https://jsbin.com/mezifegada/1/edit?js,output" rel="noopener noreferrer" target="_blank">下拉式選單切換地區資料</a>

<a href="http://jsbin.com/gadodidade/1/edit?html,console,output" rel="noopener noreferrer" target="_blank">街景服務</a>

<a href="https://developers.google.com/maps/documentation/javascript/examples/?hl=zh-tw" rel="noopener noreferrer"
        target="_blank">官方範例</a>

<a href="http://maps.vasile.ch/geomask/" rel="noopener noreferrer" target="_blank">geomask</a>
    