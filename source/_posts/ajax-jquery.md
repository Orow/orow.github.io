---
title: "[jQuery] - AJAX"
date: 2019-03-23 20:00:00
tags:
  - jQuery
categories:
  - jQuery
---

本篇 jQuery 的 AJAX 是從網路資源與網路平台`pluralsight`中的課程所學習。

## AJAX

在前面 javascript 的篇章提過 ajax，在 jquery 中也有提供 ajax 方法來達到非同步資料交換。

### AJAX settings

直接以案例來做說明：
ex.
顯示 localhost 圖片來源：
![](https://i.imgur.com/M0AIOID.png)

首先先點擊隱藏起來的機票資訊，資訊撈到後透過`slidedown()`顯示，再去點擊登機證連結後會找到`img`這個元素並`.show()`，這裡的 url 是 localhost 的絕對路徑`/'ticket-14836.png'/`。
以此案例來說，航班資訊是動態隨時，所以機票與登機證的資訊需要從網路上獲取，這時候就需要用到 AJAX 的方式了。

AJAX Method：`$.ajax(url[, settings])`

```js
// slidedown ticket information
$(".confirmation").on("click", "button", function() {
  // url也可以用relative url，等同於"confirmation.html"
  $.ajax("http://example.org/confirmation.html", {
    // success是溝通成功後執行的callback
    success: function(response) {
      $(".ticket").html(response).slideDown();
    }
  });
});
```

```html
<!-- 透過ajax帶出來的資訊 -->
<div>
  ...
  <!-- 其他內容 -->
  <strong>Boarding Pass: </strong>
  <a href="#" class="view-boarding-pass">View Boarding Pass</a>
  <img src="ticket.png" alt="Your boarding pass" class="boarding-pass" />
</div>
```

![](https://i.imgur.com/UqlZeZE.png)

- url 是可以用 relative url（相對網址）來替代的
  "http://example.org/confirmation.html" = "confirmation.html"

### shorthand with \$.get

`$.get`讓程式更簡潔。
`$.get(url, success)` = `$.ajax(url[, settings])`

使用`$.ajax`：

```js
$.ajax("confirmation.html", {
  success: function(response) {
    $(".ticket").html(response).slideDown();
  }
});
```

使用`$.get`替代：

```js
$.get("confirmation.html", function(response) {
  $(".ticket").html(response).slideDown();
});
```

### Sending parameters with requests

有兩種方式都可達到這個效果：

1. 透過 url 網址直接附上參數。
2. 在 ajax method 中使用 data 來帶入參數。

```js
//url 中的參數為confNum='1234'
$.ajax("confirmation.html?confNum=1234", {
  success: function(response) {
    $(".ticket").html(response).slideDown();
  }
});
```

```js
$.ajax("confirmation.html", {
  success: function(response) {
    $(".ticket").html(response).slideDown();
  },
  // 透過data帶入物件中的參數
  data: { confNum: 1234 }
});
```

動態（dynamic）獲取參數
一般來說，這些參數資料會是變動的，所以會使用動態的方式來取。

```html
<div class="ticket" data-confNum="1234"></div>
```

```js
$.ajax("confirmation.html", {
  success: function(response) {
    $(".ticket").html(response).slideDown();
  },
  // 用jQuery方式從dom中獲得data-confNum的參數
  data: { confNum: $(".ticket").data("confNum") }
});
```

## AJAX Options

### Handling failed AJAX requests

當發出 ajax requests 結果發生 timeout 或是 error 異常，可以使用 error 執行一些行為。
ex. 異常發生後，會 alert 提示訊息。
![](https://i.imgur.com/m9e2qZm.png)

```js
$(".confirmation").on("click", "button", function() {
  $.ajax("confirmation.html", {
    success: function(response) {
      $(".ticket").html(response).slideDown();
    },
    // 加上error處理行為，alert提示訊息
    error: function(request, errorType, errorMessage) {
      alert("Error: " + errorType + " with message: " + errorMessage);
    },
    timeout: 3000 // 設定timeout,秒數為ms
  });
});
```

### beforeSend & complete callbacks

beforeSend 是在 ajax requests 被激發前可以製作一些 loading 畫面，告訴瀏覽者需要準備要做資料傳輸需要一點時間等。
complete 是在 requests 發送完成且正常運行，或者 error 後也可以執行 callback function。
![](https://i.imgur.com/agadRLt.png)

## AJAX with Form & JSON

### AJAX with Form

透過 AJAX 去 submit form 表單的內容，需要將`$.ajax`中的 type 先改為 post，`data` 也要帶出資料的 value。

```html
<form action="/book">
  <select id="destintation" name="destination">
    ...
  </select>
  <input type="text" id="quantity" name="quantity" value="1" />
</form>
```

```js
$('form').on('submit', function(event){
  // 用preventDefault來防止submit的預設送出動作
    event.preventDefault();
    $.ajax('/book', {
        type: 'POST' // Will do a POST request to the server, which is used for forms
        // data中用object包住select&input的value
        data:{ "destination": $('#destination').val(),
                "quantity": $('#quantity').val()}
        success: function(result){
        $('form').remove();
        $('#vacation').hide().html(result).fadeIn();
        }
    });
});
```

Shortcut method - serialize()

`serialize()`可以將`<input>`, `<textarea>`, `<select>`或者`form`本身進行序列化成字符串。
但只會將『成功的控制物件(success control)』， 且要含有`name`這個屬性，才會包含序列字符串中。

ex. 以上面的程式碼為例，上下兩個結果相同

```js
data:{ "destination": $('#destination').val(),
        "quantity": $('#quantity').val()}
     }
-------------------------------------------------
data: $('form').serialize()
```

[serialize()官方文件](https://api.jquery.com/serialize/)

上面的例子 form 的 action 跟\$.ajax 的 url 相同，在盡量避免重複的情況下可以用下面的方式取代。
`attr(<attribute>), attr(<attribute>, <value>)`
![](https://i.imgur.com/c8zf0s3.jpg)

### AJAX with JSON

上一個例子 form 表單的內容要透過 AJAX 來傳送，如果內容要用 JSON 的資料傳送時，需要`$.ajax`中加上兩個參數。
`dataType:'json'`與`contentType:'application/json'`。
![](https://i.imgur.com/ufxwuN6.png)

Server return details

```html
{
    location: 'Paris',
    totalPrice: 1196.00,
    nights: 4,
    confirmation:  '345feab'
}
```

成功傳送後的 callback，因為回傳的 result 不是 html，所以要先新增一個 html tag。
![](https://i.imgur.com/llPbwqm.png)

### getJSON

`$.ajax`中選擇資料格式為 json 也可以使用更簡潔的方式：`$.getJSON`。
$.ajax(url, [, success:function(result){}]) = $.getJSON(‘url’, function(result){})

```js
$('button').on('click',function(){
    $.ajax('/status',{
        contentType: 'application/json',
        dataType: 'json',
        success: function(result){ ... }
    });
});

-------------------上下相同--------------------

$('button').on('click',function(){
    $.getJSON('/status', function(result){ ... }
    });
});

```
