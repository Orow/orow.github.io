---
title: AJAX(side project) - JavaScript
date: 2019-03-11 23:00:00
tags:
  - javascript
categories:
  - project
---

## AJAX side project
這個 side project，有註冊跟登入表單輸入。
![](https://i.imgur.com/hJWuLOG.png)

概略流程：
1. addEventListener監聽
2. function中用來取到輸入的帳號密碼的value，並將取得的帳號密碼的值塞回object中。
3. var xhr = new XMLHttpRequest()
4. xhr.open()
5. xhr.setRequestHeader()
6. xhr.send() - 內容要轉為字串
7. 用xhr.onload function (){} / 確保資料都跑完再執行function
8. 因為要判斷資料後再去執行所需的alert,在獲取資料前要先將json內格式從字串轉為陣列
9. 再利用物件中的"message"來判斷執行哪種alert

註冊表單

- 註冊成功
![](https://i.imgur.com/Yv8NvjM.png)

- 已被使用得帳號
![](https://i.imgur.com/NxOK4zw.png)



登入表單

- 登入成功
![](https://i.imgur.com/OVnjZLP.png)

- 不存在或帳號已被使用
![](https://i.imgur.com/XAvfAgy.png)


```html
<h3>註冊</h3>
帳號：
<input type="text" name="account" class="account" />
<br />
密碼：
<input type="password" name="password" class="password" />
<br />
<input type="submit" value="Register" class="send" />

<h3>登入</h3>
帳號：
<input type="text" name="account" class="accountLogin" />
<br />
密碼：
<input type="password" name="password" class="passwordLogin" />
<br />
<input type="submit" value="SignIn" class="signIn" />
```

在下面 js 中，從伺服器中比對送出的資料後，判斷格式中的 message 內容作出下一步的行為。

JSON 格式
註冊：

    Data:
    {
        email: 'lovef2e@hexschool.com',
        password: '12345678'
    }
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

登入：

    Data:
    {
        email: 'lovef2e@hexschool.com',
        password: '12345678'
    }
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


```js
// ----AJAX sample registration----
var send = document.querySelector(".send");
send.addEventListener("click", signup, false);

function signup() {
  var emailStr = document.querySelector(".account").value;
  var passwordStr = document.querySelector(".password").value;
  //把要送到伺服器的data設成物件，並將輸入的值帶進去
  var account = {};
  account.email = emailStr;
  account.password = passwordStr;

  var xhr = new XMLHttpRequest();
  xhr.open("post", "不方便公開的url-註冊", true);
  xhr.setRequestHeader("Content-type", "application/json");
  var data = JSON.stringify(account);
  xhr.send(data);
  
  xhr.onload = function() {
    var callBackData = JSON.parse(xhr.responseText);
    console.log(callBackData);
    var verifyStr = callBackData.message;
    if (verifyStr == "帳號註冊成功") {
      alert("帳號註冊成功!!");
    } else if (verifyStr == "此帳號已被使用") {
      alert("此帳號已被使用!!\n請更換帳號名稱並重新註冊");
    } else {
      alert("帳號註冊失敗!!");
    }
  };
}

// ----AJAX sample login----
var signIn = document.querySelector(".signIn");
signIn.addEventListener("click", signin, false);

function signin() {
  var emailStr = document.querySelector(".accountLogin").value;
  var passwordStr = document.querySelector(".passwordLogin").value;
  //把要送到伺服器的data設成物件，並將輸入的值帶進去
  var account = {};
  account.email = emailStr;
  account.password = passwordStr;

  var xhr = new XMLHttpRequest();
  xhr.open("post", "不方便公開的url－登入", true);
  xhr.setRequestHeader("Content-type", "application/json");
  var data = JSON.stringify(account);
  xhr.send(data);

  xhr.onload = function() {
    var callBackData = JSON.parse(xhr.responseText);
    console.log(callBackData);
    var verifyStr = callBackData.message;
    if (verifyStr == "登入成功") {
      alert("帳號登入成功!!");
    } else {
      alert("此帳號不存或帳號密碼錯誤!!");
    }
  };
}
```
