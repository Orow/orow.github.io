---
title: "[Node.js] - Express-註冊與登入驗證"
date: 2019-04-10 21:50:00
tags:
  - javascript
  - ajax
  - node.js
  - express
categories:
  - project
---

## 前言

這個 AJAX - 註冊與登入驗證的功能在前面的文章已經有練習過
先前練習的連結：[AJAX-註冊與登入驗證](https://orow.github.io/2019/03/11/ajax-js-sideproject/)
![](https://i.imgur.com/hJWuLOG.png)

當時是用網路課程的後端 url，這裡要嘗試自己用 express 的方式架設，達到一樣的功能
這裡 express 的架構是參考 Github 上 FaztWeb 的資源：[nodejs-ajax-crud](https://github.com/FaztWeb/nodejs-ajax-crud)

## Exprees 後端

主要目的用express架起這個註冊登入功能的資料庫。
分為 signup 註冊與 signin 登入。

Server 程式碼：
修改了參考資源中的 port 與 routes 的根目錄

```js
const path = require("path");
const express = require("express");
const morgan = require("morgan");
const app = express();

// settings
app.set("port", process.env.PORT || 8000);

// middlewares
app.use(morgan("dev"));
app.use(express.urlencoded({ extended: false }));
app.use(express.json());

// routes
app.use("/api", require("./routes/index"));

// static files
app.use(express.static(path.join(__dirname, "public")));

// start the server
app.listen(app.get("port"), () => {
  console.log(`server on port ${app.get("port")}`);
});
```

Routes 程式碼：

```js
const express = require("express");
const router = express.Router();

const lists = require("../data.json");

// Registration
router.post("/signup", (req, res) => {
  const infos = req.body;
  console.log(infos);
  // 比對送過來的mail不是已存在，之後再針對結果去判斷
  lists.forEach((singleList, i) => {
    if (infos.email == lists[i].email) {
      existEmail = lists[i].email;
    }
  });
 
  //已存在的帳號回傳一個含有message已被使用的物件，不存在就push回原資料中，並回傳註冊成功
  if (infos.email == existEmail) {
    console.log(lists);
    res.json({
      success: false,
      result: {},
      message: "此帳號已被使用"
    });
  } else {
    lists.push({
      id: lists.length + 1,
      email: infos.email,
      password: infos.password
    });
    console.log(lists);
    res.json({
      success: true,
      result: {},
      message: "帳號註冊成功"
    });
  }
});
// Signin
router.post("/signin", (req, res) => {
  const infos = req.body;
  console.log(infos);
  // 比對送過來的mail不是已存在，之後再針對結果去判斷
  lists.forEach((singleList, i) => {
    if (infos.email == lists[i].email) {
      existEmail = lists[i].email;
    }
  });
  //已存在的帳號回傳一個含有message已被使用的物件，不存在就push回原資料中，並回傳註冊成功
  if (infos.email == existEmail) {
    console.log(lists);
    res.json({
      success: true,
      result: {},
      message: "登入成功"
    });
  } else {
    console.log(lists);
    res.json({
      success: false,
      result: {},
      message: "此帳號不存在或帳號密碼錯誤"
    });
  }
});

module.exports = router;
```

前端 JS 概略流程則與先前課程用的方式相同：

1. addEventListener 監聽
2. function 中用來取到輸入的帳號密碼的 value，並將取得的帳號密碼的值塞回 object 中。
3. var xhr = new XMLHttpRequest()
4. xhr.open()
5. xhr.setRequestHeader()
6. xhr.send() - 內容要轉為字串
7. 用 xhr.onload function (){} / 確保資料都跑完再執行 function
8. 因為要判斷資料後再去執行所需的 alert,在獲取資料前要先將 json 內格式從字串轉為陣列
9. 再利用物件中的"message"來判斷執行哪種 alert

Server 預設資料：

```js
[
  {
    id: 1,
    email: "11@11.com",
    password: "11"
  },
  {
    id: 2,
    email: "11@22.com",
    password: "22"
  }
];
```

Signup:
Browser 發出 request 後，router 會先判斷輸入的 email 是不是已經存在，再送出對應的JSON資料。

    註冊：
    Success Response:
    {
        "success": true,
        "result": {},
        "message": "帳號註冊成功"
    }
    Error Response:
    {
        "success": false,
        "result": {},
        "message": "此帳號已被使用"
    }

註冊成功
![](https://i.imgur.com/pON5SZ5.png)

註冊失敗
![](https://i.imgur.com/nZjT46Y.png)

Signin:
發出 request 後，router 會先判斷輸入的 email 是不是已經存在，再送出對應的JSON資料。

    登入：
    Success Response:
    {
        "success": true,
        "result": {},
        "message": "登入成功"
    }
    Error Response:
    {
        "success": false,
        "result": {},
        "message": "此帳號不存在或帳號密碼錯誤"
    }

登入成功
![](https://i.imgur.com/7PGKdqM.png)

登入失敗
![](https://i.imgur.com/MO1TNyI.png)

