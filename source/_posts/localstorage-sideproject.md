---
title: "[JavaScript] - localstorage(side project)"
date: 2019-03-10 23:50:00
tags:
  - javascript
categories:
  - project
---
## localstorage side project
簡單說明一下這個side project：
- 輸入內容
- 點擊加入待辦事項
- 下方依照順序列出待辦事項，並可以點擊刪除
如下圖：
![](https://i.imgur.com/qtwNZ3Y.png)

```html
<input type="text" class="textClass" placeholder="請填寫代辦內容" />
<input type="button" class="addList" value="加入代辦" />
<ul class="list"></ul>
```

要特別注意幾點>
- 一開始要給待辦事項清單一個空陣列
- 一開始就需要更新一次清單
- 儲存與刪除清單時都要先將陣列轉為字串再setItem進localstorage
- 待辦事項清單在新增或刪除後已經是以字串的方式儲存，要調用的時候要先轉回原本陣列型別

```js
// 指定dom
var list = document.querySelector(".list");
var addList = document.querySelector(".addList");
// 待辦事項清單在新增或刪除後已經是以字串的方式儲存，要調用的時候要先轉回原本陣列型別
var data = JSON.parse(localStorage.getItem("toDoList")) || [];

// 監聽與更新
addList.addEventListener("click", saveItem);
list.addEventListener("click", deleteItem);
updateList(data);

// 加入列表,並同步更新網頁與localStorage
function saveItem(e) {
  e.preventDefault();
  var str = document.querySelector(".textClass").value;
  // 把輸入的內容設為物件並push到data陣列中
  var todo = {
    content: str
  };
  data.push(todo);
  // 加入陣列後需要更新清單
  updateList(data);
  // 更新後將資料轉為字串存到localstorage
  localStorage.setItem("toDoList", JSON.stringify(data));
}

// 更新網頁內容
function updateList(items) {
  str = "";
  var len = items.length;
  for (var i = 0; i < len; i++) {
    str += '<li><a href="#" data-index=' + i + ">刪除</a> <span>" + items[i].content + "</span></li>";
  }
  list.innerHTML = str;
}

// 刪除待辦事項
function deleteItem(e) {
  e.preventDefault();
  // 確認點擊的位置是a連結也就是"刪除"
  if (e.target.nodeName !== "A") {
    return;
  }
  var index = e.target.dataset.index;
  // 依照index的位置刪除1個item
  data.splice(index, 1);
  localStorage.setItem("toDoList", JSON.stringify(data));
  updateList(data);
}
```
