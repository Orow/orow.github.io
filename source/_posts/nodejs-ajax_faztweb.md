---
title: "[Node.js] - Express-Postman模擬"
date: 2019-04-08 21:50:00
tags:
  - javascript
  - ajax
  - node.js
  - express
  - jQuery
categories:
  - ajax
---

## 1. 後端 server 第一次的接觸

前端應用也需要了解後端 server 的運作，在交握工作能更快速的彼此了解，進而加速工作效益。
從 github 上找了一個例子利用 node.js 的框架 express 來分析如何運作。

### 1.1 簡單的 Express 伺服器

Express 伺服器說明的[參考資料](https://kopu.chat/2017/08/11/node-js-express-%E5%88%9D%E5%85%A5%E9%96%80-%E4%B8%8A%E9%9B%86/)

Express 的 server 與 router 概念目前沒有很熟悉，知道 router 是後端 server 與 client 端（Browser）中間的過程，幫助 server 解析 url、判斷 url 路徑、執行相應的動作。

簡單的 Express.js 伺服器設定，首先用 const express、宣告 express 這個變數載入 express 函式庫，再宣告 app 變數為 express function、建立一個 Express 伺服器；最後告訴伺服器聽取 3000 這個 Port。

```js
const express = require("express");
const app = express(); // 建立一個express伺服器
app.listen(3000); // 告訴server聽取3000的這個port
```

在 terminal 中用 node 執行這個 js 檔案後，在 browser 網址輸入`localhost:3000`就可以連線到伺服器。
之後伺服器就要建立接受 request 跟回傳 response 方法，下面為`get`的 request，接受成功後會回傳字串回去。

```js
// url:"/" 為根目錄
app.get("/", function(req, res) {
  res.send("Express is excellent!");
});
```

伺服器也可以回傳常見的 JSON 格式，下面 url 改為`/blog`，當路徑在`/blog`下的時候會回傳 JSON 的內容。

```js
app.get("/blog", function(req, res) {
  res.json({
    text: "Send blog page!"
  });
});
```

### 1.2 request 與 response 參數獲取

透過 req.params 取得參數
當伺服器拿到`/post/:id`這個路徑時，回傳 id 這個參數，寫作`req.params.id`。

```js
app.get("/post/:id", function(req, res) {
  res.send(req.params.id);
});
```

![](https://i.imgur.com/GWwc662.png)

透過 req.query 獲取 key-value 的值 - 用在 get
在 url 路徑後輸入`?key=value`的方式，就可以透過`req.query`的方式獲得 JSON 格式的該資料。
`res.send()`回傳資料到 browser 中。

ex. `URL:localhost:3000/hell?children=100`

```js
app.get("/hell", function(req, res) {
  res.send(req.query);
});
```

![](https://i.imgur.com/1jKNRvq.png)

透過 req.body 獲取 request 的主要內容 - 用在 post
browser 發出 post 的 request 帶有資料到伺服器，資料主體就是 req.body。

```js
app.post("/", (req, res) => {
  console.log(req.body);
  res.send(req.body);
});
```

## 2. Github 參考資料

Github：FaztWeb [nodejs-ajax-crud](https://github.com/FaztWeb/nodejs-ajax-crud)

這個案例功能為獲取 products 資料，新增資料，修改，刪除
檔案則分為三部分：Server 伺服器、Routes 路由、Public 靜態網頁。
![](https://i.imgur.com/QTyWL5B.png)

Server 檔案中不太理解裡面的內容，不過可以得知 port 是 3000。
routes 資料夾中的 index.js 的根目錄就是`/api/products`。

放上程式碼
Server:

```js
const path = require("path");
const express = require("express");
const morgan = require("morgan");
const app = express();

// settings
app.set("port", process.env.PORT || 3000);

// middlewares
app.use(morgan("dev"));
app.use(express.urlencoded({ extended: false }));
app.use(express.json());

// routes
app.use("/api/products", require("./routes/index"));

// static files
app.use(express.static(path.join(__dirname, "public")));

// start the server
app.listen(app.get("port"), () => {
  console.log(`server on port ${app.get("port")}`);
});
```

Routes 中則是先把 products 指導一個 json 檔案，代表預設的資料內容與格式。
用到了四種方法：`get`、`post`、`put`、`delete`。

```js
const express = require("express");
const router = express.Router();

const products = require("../data.json");
// data.json內容：
// [
//   {
//     "id": 1,
//     "name": "laptop"
//   },
//   {
//     "id": 2,
//     "name": "microphone"
//   }
// ]

router.get("/", (req, res) => {
  res.json(products);
});

router.post("/", (req, res) => {
  console.log(req.body);
  const { name } = req.body;
  products.push({
    id: products.length + 1,
    name
  });
  res.json("Successfully created");
});

router.put("/:id", (req, res) => {
  console.log(req.body, req.params);
  const { id } = req.params;
  const { name } = req.body;

  products.forEach((product, i) => {
    if (product.id == id) {
      product.name = name;
    }
  });
  res.json("Successfully updated");
});

router.delete("/:id", (req, res) => {
  const { id } = req.params;

  products.forEach((product, i) => {
    if (product.id == id) {
      products.splice(i, 1);
    }
  });
  res.json("Successfully deleted");
});

module.exports = router;
```

靜態網頁的 html、jQuery 則是用來操作送出資料與 ajax 的方式
[作者程式碼連結](https://github.com/FaztWeb/nodejs-ajax-crud/tree/master/src/public)

### 2.1 觀察分析 API

分析 http verb 的行為操作，這個案例用到了`get`、`post`、`put`、`delete`。

觀察程式碼後，分析出下面四個方法所做的事情

- get：獲取目前 json 資料，id=1 與 id=2 加上個別的 name 值
- post：從 postman 用 body 送出一個 json-name 值，再 get 一次確認有無新增成功
- put：url 需指到/:id 後，body 送出一個修改過的 name 值，再 get 一次確認有無修改成功
- delet：url 需指到/:id 後，直接執行後再 get 一次確認有沒有成功刪除

#### 2.1.1 Chrome Network

在 terminal 用 node 開啟 server，在 chrome 中針對不同的 verb 輸入相應的 url，在 dev tool 中的 Network 可以觀察狀態與內容。
![](https://i.imgur.com/TiAvdbK.png)

#### 2.1.2 Postman

用 Postman 模擬每一個 request 的行為，來分析這一個 API 在發出 request 後，後端獲取到什麼資訊。

**Get**
method與url輸入後按下send就可以確認結果。
![](https://i.imgur.com/TaqatCC.png)

**POST**
選擇post method，送出的資料需要是在body裡面，在server中也console出body來確認資料內容。
![](https://i.imgur.com/AiAwaKH.png)

從 route.js 還有 jQuery 的 code 來看，req.body = data
在 route.js 有看到會把新增的 id 還有 req.body 不含大括號的部分 push 回 data.json 的檔案中

```js
// route.js
router.post("/", (req, res) => {
  console.log(req.body);
  const { name } = req.body;
  products.push({
    id: products.length + 1,
    name
  });
  res.json("Successfully created");
});
```

```js
// jQuery
// POST PRODUCTS
$("#productForm").on("submit", e => {
  e.preventDefault();
  let newProduct = $("#newProduct");

  $.ajax({
    url: URI,
    method: "POST",
    data: {
      name: newProduct.val()
    },
    success: function(response) {
      newProduct.val("");
      $("#getProducts").click();
    },
    error: function(err) {
      console.log(err);
    }
  });
});
```

**PUT**
put就是修改原有資料的意思，這裡是依照 id，url 要改為`/:id`
在 route 中用 forEach + if 判斷來重新確認點擊 update 按鈕的該 id 要相同後，再去修改 name 值。

```js
// route
router.put("/:id", (req, res) => {
  console.log(req.body, req.params);
  const { id } = req.params;
  const { name } = req.body;

  products.forEach((product, i) => {
    if (product.id == id) {
      product.name = name;
    }
  });
  res.json("Successfully updated");
});
```

jQuery 中則是要先更新修改後的 value 到 data 裡面，成功後的 call back 會在執行 get 的事件。

```js
// jQuery
$("table").on("click", ".update-button", function() {
  let row = $(this).closest("tr");
  let id = row.find(".id").text();
  let name = row.find(".name").val();

  $.ajax({
    url: `${URI}/${id}`,
    method: "PUT",
    data: {
      name: name
    },
    success: function(response) {
      console.log(response);
      $("#getProducts").click();
    }
  });
});
```

原本 id = 3 時，name=1，把 name改為3333333，再透過put的方式去修改後端資料。
![](https://i.imgur.com/1joHKh8.png)

再用 get 確認
![](https://i.imgur.com/ZmsXypg.png)

**Delete**
Delete method也就是刪除，這裡是依照 id，url 要改為/:id
在 route 中也是用 forEach + if 判斷來重新確認點擊 deleye 按鈕的該 id 要相符後用 splice 來刪除 server 上的資料。

```js
// route
router.delete("/:id", (req, res) => {
  const { id } = req.params;

  products.forEach((product, i) => {
    if (product.id == id) {
      products.splice(i, 1);
    }
  });
  res.json("Successfully deleted");
});
```

```js
// jQuery
$("table").on("click", ".delete-button", function() {
  let row = $(this).closest("tr");
  let id = row.find(".id").text();

  $.ajax({
    url: `${URI}/${id}`,
    method: "DELETE",
    success: function(response) {
      $("#getProducts").click();
    }
  });
});
```

執行 delete
![](https://i.imgur.com/Jld8kZn.png)

get 確認
![](https://i.imgur.com/pUfXmow.png)
