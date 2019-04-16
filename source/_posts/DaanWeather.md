---
title: "[JavaScript] - AJAX：氣象即時資訊"
date: 2019-04-06 21:18:43
tags:
  - javascript
  - ajax
category:
  - project
---

## Demo

JS-AJAX 練習-氣象即時資訊- [Demo](https://orow.github.io/MyProjects/HandsOnJS-JSON/05-tw-weather/index.html)

## 1. Introduction 介紹

JS 的 AJAX 練習，使用第三方 API 介接，顯示台北市大安區現在溫度與三天內的氣象資訊。
![](https://i.imgur.com/pqBBDlB.png)

- 介接第三方資訊 - 中央氣象局開放資料平台
- 撈取位置，觀測站別、目前溫度、風速
- 三天內的天氣現象、溫度、6 小時降雨機率

[中央氣象局 API 指南](https://opendata.cwb.gov.tw/devManual/insrtuction)
[中央氣象局開放資料平臺之資料擷取 API](https://opendata.cwb.gov.tw/dist/opendata-swagger.html#/)
[自動氣象站資料及說明](https://opendata.cwb.gov.tw/opendatadoc/DIV2/A0001-001.pdf)

## 2. 功能

使用了中央氣象局開方資料平台中的兩個資料 API，一個是氣象站-現在天氣觀測報告，另一個則是台北市未來兩天天氣預報。
在中央氣象局的資料平台網站中，可以設定一些條件後執行，馬上可以看到回傳的資料是哪些。
![](https://i.imgur.com/hUHSDuB.png)

這邊使用到兩個資料 API，所以會宣告兩個`new XMLHttpRequest()`，分左右區塊來說明。

左邊區塊顯示的是現在天氣

- 位置資訊
- 站別名稱
- 現在溫度
- 風速

![](https://i.imgur.com/SLOmLhm.png)

```html
<section id="left">
  <div id="location">City UT</div>
  <div id="station">Current station</div>
  <div id="temperature">00</div>
  <div id="desc">A mix of clouds</div>
</section>
```

```js
var weatherConditions = new XMLHttpRequest();
var cObj;

// GET THE CONDITIONS
weatherConditions.open("get", "現在天氣觀測的url", true);
weatherConditions.send();

weatherConditions.onload = function() {
  if (weatherConditions.status === 200) {
    cObj = JSON.parse(weatherConditions.responseText);
    console.log(cObj);

    // area of taipei
    var taipeiDaan =
      cObj.records.location[0].parameter[0].parameterValue + " " + cObj.records.location[0].parameter[2].parameterValue;

    // temperature
    var daanTemp = cObj.records.location[0].weatherElement[3].elementValue;

    // Wind Speed
    var windSpeed = cObj.records.location[0].weatherElement[2].elementValue;

    document.getElementById("location").innerHTML = taipeiDaan;
    document.getElementById("station").innerHTML = "站別：" + cObj.records.location[0].locationName;
    document.getElementById("temperature").innerHTML = daanTemp;
    document.getElementById("desc").innerHTML = "Wind Speed: " + windSpeed;
  }
};
```

右邊是未來兩天天氣預報，這邊每一列是分開處理，第一列是標題，第二列以後只有取的值的 index 不同。
用陣列index搭配object的選取，從中取出資料加回html網頁中。

- 日期
- 天氣現象
- 溫度
- 6 小時降雨機率
  ![](https://i.imgur.com/0a2h2nX.png)

```html
<header>
  <!-- 最上面的Title -->
  <h1 id="area">My Weather Channel</h1>
  <h2>JSON data from 中央氣象局</h2>
</header>

<section id="right">
  <div>
    <span id="r1c1"></span>
    <span id="r1c2"></span>
    <span id="r1c3"></span>
    <span id="r1c4"></span>
  </div>
  <div>
    <span id="r2c1"></span>
    <span id="r2c2"></span>
    <span id="r2c3"></span>
    <span id="r2c4"></span>
  </div>
  <div>
    <span id="r3c1"></span>
    <span id="r3c2"></span>
    <span id="r3c3"></span>
    <span id="r3c4"></span>
  </div>
  <div>
    <span id="r4c1"></span>
    <span id="r4c2"></span>
    <span id="r4c3"></span>
    <span id="r4c4"></span>
  </div>
</section>
```

```js
var weatherForecast = new XMLHttpRequest();
var fObj;
// GET THE FORECARST
weatherForecast.open("get", "未來兩天天氣預報的url", true);
weatherForecast.send();

weatherForecast.onload = function() {
  if (weatherForecast.status === 200) {
    fObj = JSON.parse(weatherForecast.responseText);
    console.log(fObj);
    // title
    var area_raw = fObj.records.locations[0].datasetDescription;
    area_raw = area_raw.substring(7, 17) + area_raw.substring(21);

    // weather situation
    var weatherSituation =
      fObj.records.locations[0].location[0].weatherElement[1].description;

    // temperature
    var currentTemp =
      fObj.records.locations[0].location[0].weatherElement[3].description;

    // rainPrecent
    var rainPrecent =
      fObj.records.locations[0].location[0].weatherElement[7].description;

    document.getElementById("area").innerHTML = "臺北市大安區" + area_raw;
    document.getElementById("r1c1").innerHTML = "日期";
    document.getElementById("r1c2").innerHTML = weatherSituation;
    document.getElementById("r1c3").innerHTML = currentTemp;
    document.getElementById("r1c4").innerHTML = rainPrecent;

    // date
    var date_raw = fObj.records.locations[0].location[0].weatherElement[1].time[3].startTime;
    date_raw = date_raw.substring(5, 11);

    // weather situation
    var weatherSituation =
      fObj.records.locations[0].location[0].weatherElement[1].time[3].elementValue[0].value;

    // temperature
    var currentTemp =
      fObj.records.locations[0].location[0].weatherElement[3].time[3].elementValue[0].value;

    // rainPrecent
    var rainPrecent =
      fObj.records.locations[0].location[0].weatherElement[7].time[2].elementValue[0].value;

    document.getElementById("area").innerHTML = "臺北市大安區" + area_raw;
    document.getElementById("r2c1").innerHTML = date_raw;
    document.getElementById("r2c2").innerHTML = weatherSituation;
    document.getElementById("r2c3").innerHTML = currentTemp + "&deg";
    document.getElementById("r2c4").innerHTML = rainPrecent + "%";
  }
};
```
