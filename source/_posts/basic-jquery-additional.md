---
title: "[jQuery] - Basic（補充）"
date: 2019-03-24 20:00:00
tags:
  - jQuery
categories:
  - jQuery
---

本篇 jQuery 的 AJAX 是從網路資源與網路平台`pluralsight`中的課程所學習。

## Utility Methods

### $.each

透過`$.each()`把陣列或物件中的內容重複取出，再依次去執行 callback。
![](https://i.imgur.com/bBZSZQr.png)

以下案例為：點擊一個 button，要秀出全部喜愛的城市與圖片。

html

```html
<div class="favorite-0">
  ...
</div>
<div class="favorite-1">
  ...
</div>
<div class="favorite-2">
  ...
</div>
```

result 的內容（JSON 格式）

```js
{
    image: 'imgaes/paris.png',
    name: 'Paris, France',
},
{
    image: 'imgaes/london.png',
    name: 'London, UK',
},
{
    image: 'imgaes/mardird.png',
    name: 'Madrid, Spain',
}
```

用`$.each`來迭代 result 這個 JSON 的物件，並將 name 與 image 新增到 html 內容裡面。

```js
$("button").on("click", function() {
  $.ajax("/cities/favorite/1", {
    contentType: "application/json",
    dataType: "json",
    success: function(result) {
      // resutl是json object
      $.each(result, function(index, city) {
        var favorite = $(".favorite-" + index);
        favorite.find("p").html(city.name);
        favorite.find("img").attr("src", city.image);
      });
    }
  });
});
```

[$.each 官方文件](https://api.jquery.com/jQuery.each/)

### $.map

`$.map`
![](https://i.imgur.com/BnWFm2q.png)

使用 map 創立新數組
ex.1
![](https://i.imgur.com/cnWRZfS.png)

ex.2
![](https://i.imgur.com/BMliVFI.png)

將 return 後的 listItem 指定為`statusElements`再塞回整個清單中。
![](https://i.imgur.com/LkGArRj.png)

與`$.each`的差異是 map 會把結果以新的數組 return，each 沒有返回值，不會改變其值。
![](https://i.imgur.com/2OhKquA.png)

[$.map 官方文件](http://api.jquery.com/jquery.map/)

### detach

![](https://i.imgur.com/pVJYhDQ.png)

使用 detach 而不使用 remove 來刪除元素，detach 會把選取的元素移除，但是會保留著，還是可以再去使用。
這是一個更有效能的做法，

在上面的例子中改為：

```js
$(".status-list")
  .detach()
  .html(statusElements)
  .appendTo(".status");
```

[detach 官方文件](https://api.jquery.com/detach/)

## Advanced Events

### off

停止去監聽符合條件的事件（通常是用 on 新增的事件）。
![](https://i.imgur.com/CFw9fUC.png)

ex.1 停止全部 click 事件
![](https://i.imgur.com/nebEntN.png)

ex.2 停止單一事件 - 需要應用到 namespacing
![](https://i.imgur.com/1Yez98f.png)

ex.3 停止全部相同 namespace 的事件
![](https://i.imgur.com/3KsU8WR.png)

### trigger

`trigger()`用來觸發所指定的事件類型。

ex1. 等同於點擊 button 觸發事件
同時觸發兩個 click 事件

```js
function picture(){console.log('Show Plane');}
function status(){console.log('In Service');}

    $(document).ready(function(){
        $('button').on('click.image', picture);
        $('button').on('click.details', status);
        $('button').on('mouseover.image', zoom);
        ...
        $('button').trigger('click');
    });
```

ex.2 trigger 也可以指定 namespace 來觸發
觸發 image namespace 的 click 事件

```js
function picture(){console.log('Show Plane');}
function status(){console.log('In Service');}

    $(document).ready(function(){
        $('button').on('click.image', picture);
        $('button').on('click.details', status);
        ...
        $('button').trigger('click.image');
    });
```

ex.3 可以trigger自訂的event.namespace
trigger一個按鈕可以觸發所有符合條件項目的function
![](https://i.imgur.com/cjtK7Vi.png)

![](https://i.imgur.com/j9WxVoS.jpg)

