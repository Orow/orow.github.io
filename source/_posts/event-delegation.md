---
title: "[javascript] - event delegation"
date: 2019-03-20 14:00:00
tags:
  - jQuery
  - javascript
categories:
  - javascript
---
此篇概念參照網路課程`pluralsight`及網路資源。

## event delegation

這是用來減少監聽的數目，但可以利用 bubbling 的方式來傳遞給實際想取得的元素的方式。
在 javascript 與 jQuery 中都各有方式可以使用。
利用 delegation 的概念可以針對動態新增的元素去執行綁定事件。

 舉例來說：

```js
$("#test [type=button]").on("click",function(){...});
```

```js
// event delegation
$("#test").on("click","[type=button]",function(){...});
```

上面兩種方式都可以綁定到`id="text"`中的`type="button"`的元素，但透過動態新增到`#test`中的`type="button"`在原本的綁定在執行 click 時，則不會觸發 function。

網路上範例：
[參考來源](http://cythilya.blogspot.com/2015/07/javascript-event-delegation.html)

當我們click不同的小區塊時，就會console出它們個別的名字，例如：a、b或c。

實作方法是將click事件綁在parent上，藉由Event Bubbling來傳遞給child，而非直接將事件綁定在child上。優點是可減少監聽器的數目，缺點是由於需要判斷哪些child node是我們有興趣的項目，而必須多寫一些程式碼做判斷。在本例中，我們加上一個filter`.child`，表示只對有`.child`這個class的節點有興趣，而沒有加上`.child`的節點則不被影響，例如click`.subitem`這個節點之後就不會console它的名字。


```html
<div class="parent">
  <div class="child" data-name="a"></div>
  <div class="child" data-name="b"></div>
  <div class="child" data-name="c"></div>
  <div class="subitem" data-name="d"></div>
</div>
```

```js
$(".parent").on("click", ".child", function() {
  console.log($(this).data("name"));
});
```

[jQuery參考](https://harttle.land/2015/06/26/jquery-event.html)
[delegate與binding比較](https://ithelp.ithome.com.tw/articles/10120565)
[Event delegation tutorial](http://javascript.info/event-delegation)