title: text
---

# Javascript - 六角：入門篇-學徒的試煉

###### tags: `javascript`

## 5. Function(函式)
#### Hositing 概念
1. 程式由上到下執行,但是如果是function => 程式會自動先把function跑完再繼續

#### ParseInt
1. string convert to number

## 7. Control & Judgement 控制判斷(運算子,if,switch)

### if, else if / switch 差異
1. if, else if 會判斷後再去執行 
2. switch 一定會執行
3. switch效能比好(先比對case才執行)

## 8. Loop 迴圈 

### i++ / i+=1
1. i++ => 先執行再++
2. i+=1 => 直接加
=>基本上結果是相同

### length
1. 對陣列
2. 物件的話無法用length

## 9. DOM ducoment object modal
### querySelector => 只選取第一筆資料
1. document.getElementById('titleId') = document.querySelector('#titleId')
    1. querySelector也可以用來選clas
        => querySelector('.titleClass')
    2. 也可以選class下的em
        => querySelector('.titleClass em')

### querySelectorAll => 選取多筆資料,以陣列(array)顯示 
### setAttribute =>修改attribute屬性內容或是新增
1. 修改

    ```js
    var el = document.querySelector('.titleClass a');
    el.setAttribute('href','https://orow.github.io/MyProjects/'); 
    ```
        
2. 新增
    下面例子為新增ID後就可以用css進行修飾

    ```js
    var el2 = document.querySelector('titleClass a');
    el.setAttribute('id','strId'); 
    ```

### innerHTML + for Loop

1. 要跑for迴圈且每次塞的html都要存在的話,先給個var =  空字串,最後再用+=保留上一次迴圈產稱的內容

### createElement
1. Example
    =>先用createElement('em')新增em
    =>再去看要修改(textContent) & 設定屬性（setAttribute)
    =>createElement後要用appendChild()加進去指定的位置

    ***appendChild()+ querySelectorAll() + for***
    =>用for迴圈加上createElement加appendChild,要加上的元素每一個都是一樣的話
    =>如果用這個方式,appendChild只會顯示最後一個,因為如果在用appendChild的時候所要加的元素已經有了,會變成用搬移的方式而不是新增複製
    =>要複製的話要在appendChild前先使用cloneNode(true)的方式
    ref: https://pjchender.blogspot.com/2017/06/js-node-element-appenchild-disappear
    
    ***appendChild()+ querySelector()***
    =>用for迴圈,每個加入的內容不一樣,視情況,基本上不需要querySelectorAll 

    ```
    #### 用Javascript操控HTML的兩個方法
    1. innerHTML
        1. 方法：組完字串後,傳進語法進行渲染
        2. 優點：效能快
        3. 缺點：資安風險,要確保來源沒問題
    2. createElement
        1. 方法：以DOM節點來處理
        2. 優點：安全性高
        3. 缺點：效能差
    ```

### XSS - cross-site scripting  - 跨站指令攻擊
1. innerHTML容易被塞script東西進來
    
    
## 10. Event

### event物件 - 告知當下元素資訊
1. function()的()可以給個名稱,資訊會帶到這個名稱裡面

```js
var el = document.querySelector('.btn');
el.onclick = function(e){
    console.log(e);  //<= 這個console.log會帶出一個objecta內容是詳細資訊
    alert('Hello')
}
```

### addEventListener - 監聽

```js
var el = document.querySelector('.btn');
                   //'選擇事件','帶入匿名function','false'
el.addEventListener('click',function(e){
    alert("Hello");
},false)
```

### onclick & addEventListener事件綁定的差異
1. 同時綁定多個事件時
    1. onclick只會顯示最後一個的事件
    2. addEventListener會全部都顯示

        ```js
        // onclick只會顯示最後一個的事件
        var elOn = document.querySelector('.btnOn');
        elOn.onclick = function(){
            alert('onclick-1');
        }
        elOn.onclick = function(){
            alert('onclick-2');
        }

        // addEventListener會兩個事件都觸發到
        var elAdd = document.querySelector('.btnAdd');
        elAdd.addEventListener('click',function(){
            alert('add-1');
        },false)
        elAdd.addEventListener('click',function(){
            alert('add-2');
        },false)
        ```
        
### event bubbling & capturing - 事件冒泡與事件捕捉
1. addEventListenter('click',function(){},false)
    1. ()中為('事件',function,false)中的第三欄為true or false(default:false)
        1. false = 從指定元素往外找 - event bubbling
        2. true = 從最外面往裡面找指定元素 - event capturing

        ```html
        <body class="body">
            <div class="box"></div>
        </body>
        ```

        ```js
        var elbox = document.querySelector('.box');
        elbox.addEventListener('click',function(){
            alert('hello box');
            console.log('box');
        },false);

        var elbody =document.querySelector('.body');
        elbody.addEventListener('click',function(){
            alert('hello body');
            console.log('body');
        },true);
        ```
        
        上述例子
        點擊box後會先alert hello body => 再alert hello box
        *如果兩個都是false,點擊box後會先alert box再alert body*
        
### stop propagation - 停止傳遞
1. 想要停止bubbling or capturing需要用到這個方式
2. 下面例子是點擊box不要再往上傳body

    ```js
    var elbox = document.querySelector('.box');
    elbox.addEventListener('click',function(e){ //需要要給值代元素資訊
        e.stopPropagation();   //再去執行停止傳遞
        alert('hello box');
        console.log('box');
    },false);

    var elbody =document.querySelector('.body');
    elbody.addEventListener('click',function(){
        alert('hello body');
        console.log('body');
    },false);
    ```
    
### preventDefault - 取消預設觸發行為
1. 要把元素資訊先待到function中在執行,如下面例子

    ```html
    <a href="https://orow.github.io/MyProjects/" class="link">Orow Projects</a>
    ```

    ```js
    var el = document.querySelector('.link');
    el.addEventListener('click',function(e){
        // 取消默認的行爲
        // 這邊是取消點擊a連結後跳轉到指定頁面

        // 可以用在submit按鈕,送出到後端前先在local端驗證有沒有資料
        e.preventDefault();
    },false);
    ```
    
### e.target - 了解目前所在的元素位置
1. 案例如下（點擊後了解到點到了那一個元素）

    ```html
    <div class="header">
            <ul style="padding-top:100px;border:1px solid #333">
                <li>
                    <a href="#">1111</a>
                </li>
            </ul>
        </div>
    ```

    ```js
    var el = document.querySelector('.header');
    el.addEventListener('click',function(e){
        console.log(e.target);
        // target中還有各種語法,如nodeName是大寫的節點名稱
    },false);
    ```

2. 六角範例：http://jsbin.com/soqegihuvu/1/edit?html,css,js,console,output

### change - 表單內容更動時觸發
1. 住在哪一區的農夫有誰呢？

```html
<body class="body">
    <select id="areaId">
        <option value=松山區>松山區</option>
        <option value=中山區>中山區</option>
        <option value=信義區>信義區</option>
    </select>
    <ul class="list"></ul>
    <script src="js/all.js"></script>
</body>
```

```js
var area = document.getElementById('areaId');
var list = document.querySelector('.list');
var county =[
    {
        farmer:'哈維',
        place:'松山區'
    },
    {
        farmer:'傑森',
        place:'中山區'
    },
    {
        farmer:'戴爾',
        place:'松山區'
    },
    {
        farmer:'威廉',
        place:'信義區'
    },
    {
        farmer:'喬治',
        place:'中山區'
    },
]
var countyLen = county.length;
function updateList(e){
    var select = e.target.value;
    var str = "";
    for (var i = 0 ; i < countyLen ; i++){
        if (select == county[i].place){
            str += '<li>'+county[i].farmer+'</li>'
        }
    }
    list.innerHTML = str;
}
area.addEventListener('change',updateList,false);
```

## 11. localStorage - included side project

### setItem & getItem
1. 設定進去與取得

    ```html
    <body>
        <h2>請輸入你的姓名</h2>
        <input type="text" class="textClass">
        <input type="button" class="btnClass" value="點擊">
        <input type="button" class="btnCall" value="點擊呼叫名字">

        <script src="js/all.js"></script>
    </body>
    ```
    
    ```js
    var btn = document.querySelector('.btnClass');
    function saveName(e){
        var str = document.querySelector('.textClass').value;
        localStorage.setItem("myName",str);
    }
    btn.addEventListener('click',saveName);

    var call =document.querySelector('.btnCall');
    function callName(e){
        var str = localStorage.getItem('myName');
        alert(str);
    }
    call.addEventListener('click',callName);
    ```

### array to string / string to array
1. 因為localStorage只會保存string資料
    1. JSON.stringify =>轉成字串
    2. JSON.parse => 字串轉陣列(object)

    ```js
    var county = [
        {farmer : '哈維'}
    ];

    var countyString = JSON.stringify(county)
    console.log(countyString);
    localStorage.setItem('countyItem',countyString);

    var getData = localStorage.getItem('countyItem');
    console.log(getData);

    var getDataArray = JSON.parse(getData);
    console.log(getDataArray[0].farmer);
    ```

### dataset - 抓取資料
1. 在localStorage中抓資料

    ```html
    <ul class="list">
        <li data-num="1" data-dog="3" class=listli>哈維</li>
    </ul>
    ```

    ```js
    var dataSet = document.querySelector('.listli').dataset;
    console.log(dataSet);

    var dataSetDetail1 = document.querySelector('.listli').dataset.num;
    console.log(dataSetDetail1);

    var dataSetDetail2 = document.querySelector('.listli').dataset.dog;
    console.log(dataSetDetail2);

    var listLi = document.querySelector('.listli');
    // 確認點擊的人名以及相關資訊

    function checkList(e){
        var num = e.target.dataset.num;
        var dog = e.target.dataset.dog;
        console.log('第' + num + '個人');
        console.log('有'+ dog +'隻狗');
    }

    listLi.addEventListener('click',checkList,false);
    ```
    
### dataset & array 運用
1. example

    ```html
    <ul style="padding:50px;background:#000;color:#fff" class="list"></ul>
    ```

    ```js
    var county = [
        {
          farmer:'卡斯伯'
        }
        ,{
          farmer:'查理'
        }
      ]
      var list = document.querySelector('.list');

      //更新農場資料
      function updateList(){
        var str = '';
        var len = county.length;
        for(var i = 0;len>i;i++){
          str+='<li data-num='+i+'>'+county[i].farmer+'</li>'
        }
        list.innerHTML = str;
      }
      updateList();

      //確認點擊的農夫是誰
      function checkList(e){
         var num = e.target.nodeName;
         if (num !== "LI"){return}
         var str = e.target.dataset.num;
         alert('現在所選擇的農夫是'+ county[str].farmer);
      }

      list.addEventListener('click',checkList,false);
    ```
    
### splice delete from array
1. 新增li進去ul後要更新資料
2. 刪除後也要更新資料

    ```html
    <ul style="padding:50px;background:#000;color:#fff" class="list"></ul>
    ```

    ```js
    var county = [{
      farmer: '卡斯伯'
    }, {
      farmer: '查理'
    }, {
      farmer: '哈維'
    }]
    var list = document.querySelector('.list');

    //更新農場資料
    function updateList() {
      var str = '';
      var len = county.length;
      for (var i = 0; len > i; i++) {
        str += '<li data-num=' + i + '>' + county[i].farmer + '</li>'
      }
      list.innerHTML = str;
    }
    updateList();

    //確認點擊的農夫是誰
    function checkList(e) {
      var num = e.target.dataset.num;
      if (e.target.nodeName !== "LI") {
        return
      }
      county.splice(num, 1);
      updateList();
    }

    list.addEventListener('click', checkList, false);
    ```
    
### localStorage - side project
1. 輸入代辦內容點擊後,新增list並產生刪除按鈕
2. 點擊刪除即從localStorage刪除資料
3. 一開始先更新欲新增的list(一開始先給空陣列)
4. 加入list後也要更新
5. 刪除後也要更新

    ```html
    <input type="text" class="textClass" placeholder="請填寫代辦內容">
        <input type="button" class="addList" value="加入代辦">
        <ul class="list"></ul>
    ```

    ```js
    // 指定dom
    var list = document.querySelector('.list');
    var addList = document.querySelector('.addList');
    var data = JSON.parse(localStorage.getItem('toDoList')) || [];

    // 監聽與更新
    addList.addEventListener('click', saveItem);
    list.addEventListener('click', deleteItem);
    updateList(data);

    // 加入列表,並同步更新網頁與localStorage
    function saveItem(e) {
      e.preventDefault();
      var str = document.querySeector('.textClass').value;
      var todo = {
        content : str
      };
      data.push(todo);
      updateList(data);
      localStorage.setItem('toDoList', JSON.stringify(data));
    }

    // 更新網頁內容
    function updateList(items){
      str="";
      var len = items.length;
      for (var i = 0 ; i < len ; i ++){
        str += '<li><a href="#" data-index=' + i + '>刪除</a> <span>' + items[i].content + '</span></li>';
      } 
      list.innerHTML = str;
    }

    // 刪除待辦事項
    function deleteItem(e){
      e.preventDefault();
      if (e.target.nodeName !== 'A'){return};
      var index = e.target.dataset.index;
      data.splice(index, 1);
      localStorage.setItem('toDoList', JSON.stringify(data));
      updateList(data);
    }
    ```
    
## 13. JS開發邏輯思維影片

### 要規劃或是參考別人程式碼
1. 資料處理
2. 事件 - 使用者有互動
3. 介面處理

## 15. AJAX - included side project

### 概論
1. `非同步資料交換` 瀏覽器不需重新整理就可以跟後端程式或是資料庫傳送接收資料 
2. 網路上說明: https://www.ithome.com.tw/node/33060
    AJAX並非是一個技術（Technology），而是一種網站設計的架構（Architecture），雖然主要以JavaScript與XML為主，但還包括其它成員，也就是CSS、DOM（Document Object Model）與HTML等，特別是XMLHttpRequest元件，使AJAX能達到`非同步資料交換`的目的。在Jesse Garrett的文章中，對AJAX的定義如下：

    ● 使用XHTML與CSS作為展現標準
    ● 使用DOM作為動態顯示與互動
    ● 使用XML與XSLT作為資料交換與運用
    ● 使用XMLHttpRequest作為非同步的資料回饋
    ● 使用JavaScript結合以上所有結果

### XMLHttpRequest跨瀏覽器撈取資料
1. new XMLHttpRequest()後,確認readtState的狀態,
2. 用open()打開
3. send()送空值=讀取值,案例如下

```js
var xhr =new XMLHttpRequest();

// readtState 
// 0 - 已經產生一個XMLHttpRequest,但還沒有連結要撈的資料
// 1 - 用了open()，但還沒有把資料傳送過去
// 2 - 偵側到用了send
// 3 - loading
// 4 - 撈到資料,數據完全接收到了

// 格式,要讀取的網址,同步與非同步
// 格式: get(讀取), post(傳送資料到伺服器)
xhr.open('get','這裡放網址',true);
    // 空值
xhr.send(null);
```

### AJAX 非同步概念
1. xhr.open('get','這裡放網址',true)
    =>裡面的true or false代表
    1. true - 非同步,不會等資料傳回來就讓程式繼續往下跑,等到資料跑完才自動回傳
    2. false - 同步,會等資料跑完回傳回來,程式再繼續往下跑

2. true + onload event
    1. xhr.onload = function (){}
        =>onload代表整個資料傳送回來接收到了(readyState:4),之後才會去執行
    2. 抓取的responseText是字串,用JSON.parse改回陣列後用for迴圈取出加回html 
        1. 建立XMLHttpRequest
        2. 傳送到對方伺服器並要資料
        3. 回傳到自己的瀏覽器
        4. 拿到資料後再看要怎麼處理

        ```js
        var xhr =new XMLHttpRequest();

        xhr.open('get','https://data.kcg.gov.tw/dataset/a98754a3-3446-4c9a-abfc-58dc49f2158c/resource/48d4dfc4-a4b2-44a5-bdec-70f9558cd25d/download/yopendata1070622opendatajson-1070622.json',true);

        xhr.send(null);

        xhr.onload = function (){
            console.log(xhr.responseText);
            var str = JSON.parse(xhr.responseText);
            for (var i = 0 ; i < str.length; i++){
                var kind = str[i].Kind;
                if ( kind == "公共充電站"){
                    document.querySelector('.list').innerHTML += '<li>'+ str[i].Location + '</li>';
                }
            }
        }
        ```
        
### HTTP狀態碼

1. chrome -> insepect -> network
    1. status = 200 資料有正確回傳,有撈到
    2. status = 404 資料讀取錯誤,沒有撈到
2. 可以利用if 判斷式(xhr.status == 200)來確認有沒有撈到資料

### CORS cross origin resource sharing

1. 有些網站沒有cors就無法取得資料
2. 測試網站能不能獲取資料:http://www.test-cors.org/

### 傳統表單輸入

1. form => input
    1. 按下
    2. input type="submit"送出後
    3. 網址列最後會在form action="目標網址"後有個?號帶出form裡面input裡面name的內容加上所輸入的資料
    4. `..../index.html?account=123&password=456`

```html
<form action="index.html">
        帳號：
        <input type="text" name="account">
        密碼：
        <input type="password" name="password">
        <input type="submit" value="confirm">
        
    </form>
```

### 註冊帳號(post)流程
1. open('post','url',true);
    =>post, 傳送資料到伺服器
    =>url,這邊填要傳過去的目標api網址
2. open()後要執行.setRequestHeader,發送request並告知格式
    1. 常用的傳統表單樣式
        =>.setRequestHeader("Content-type","application/x-www-form-urlencoded")
    2. json格式
        =>.setRequestHeader("Content-type","application/json")
3. send(),裡面格式要看目標api的格式,必須為字串
    1. 常用的傳統表單樣式
        =>('email=qqq@gamil.com&password=222')
    2. json格式,轉為字串
        =>var data = JSON.stringify(account);
        =>xhr.send(data);
        

### AJAX samples

#### Registration & Login
1. addEventListener 監聽
2. function =>取到輸入的帳號密碼的value
3. 將取得的帳號密碼的值塞回object中
4. var xhr = new XMLHttpRequest()
5. xhr.open() , xhr.setRequestHeader()
6. 目標資料庫：https://github.com/hexschool/nodejs_ajax_tutorial
    1. Signup - URL: https://hexschool-tutorial.herokuapp.com/api/signup
    2. Signin - URL: https://hexschool-tutorial.herokuapp.com/api/signin
7. xhr.send() / 內容要轉為字串
8. 用xhr.onload function (){} / 確保資料都跑完再執行function
9. 因為要判斷資料後再去執行所需的alert,在獲取資料前要先將json內格式從字串轉為陣列
10. 再利用物件中的"message"來判斷執行哪種alert

    ```html
    <h3>註冊</h3>
    帳號：
    <input type="text" name="account" class="account">
    <br>
    密碼：
    <input type="password" name="password" class="password">
    <br>
    <input type="submit" value="Register" class="send">

    <h3>登入</h3>
    帳號：
    <input type="text" name="account" class="accountLogin">
    <br>
    密碼：
    <input type="password" name="password" class="passwordLogin">
    <br>
    <input type="submit" value="SignIn" class="signIn">
    ```
    
    ```js
    // ----AJAX sample registration----
    var send = document.querySelector('.send');
    send.addEventListener('click', signup, false);

    function signup() {
        var emailStr = document.querySelector('.account').value;
        var passwordStr = document.querySelector('.password').value;
        var account = {};
        account.email = emailStr;
        account.password = passwordStr;

        var xhr = new XMLHttpRequest();
        xhr.open('post', 'https://hexschool-tutorial.herokuapp.com/api/signup', true)
        xhr.setRequestHeader('Content-type', 'application/json')
        var data = JSON.stringify(account);
        xhr.send(data);
        // 因為var xhr寫在function中是區域變數,所以function執行完後,單獨執行xhr會變成not defined  
        xhr.onload = function () {
            var callBackData = JSON.parse(xhr.responseText);
            console.log(callBackData);
            var verifyStr = callBackData.message;
            if (verifyStr == '帳號註冊成功') {
                alert('帳號註冊成功!!');
            } else if (verifyStr == '此帳號已被使用') {
                alert('此帳號已被使用!!\n請更換帳號名稱並重新註冊');
            } else {
                alert('帳號註冊失敗!!');
            }
        }
    }

    // ----AJAX sample login----
    var signIn = document.querySelector('.signIn');
    signIn.addEventListener('click', signin, false);

    function signin() {
        var emailStr = document.querySelector('.accountLogin').value;
        var passwordStr = document.querySelector('.passwordLogin').value;
        var account = {};
        account.email = emailStr;
        account.password = passwordStr;

        var xhr = new XMLHttpRequest();
        xhr.open('post', 'https://hexschool-tutorial.herokuapp.com/api/signin', true)
        xhr.setRequestHeader('Content-type', 'application/json')
        var data = JSON.stringify(account);
        xhr.send(data);
        // 因為var xhr寫在function中是區域變數,所以function執行完後,單獨執行xhr會變成not defined  
        xhr.onload = function () {
            var callBackData = JSON.parse(xhr.responseText);
            console.log(callBackData);
            var verifyStr = callBackData.message;
            if (verifyStr == '登入成功') {
                alert('帳號登入成功!!');
            } else {
                alert('此帳號不存或帳號密碼錯誤!!');
            }
        }
    }
    ```

## Google Maps API
### hackMD紀錄 - 基本API key新增與經緯度取用
ref: https://hackmd.io/vq927Yc0SVGnxCQ76sER9A

### 評估API可行性
1. API例子
    1. Google Map
    2. HEROKU - 主機服務
    3. FireBase - 用js儲存資料庫的資料(object or Array)
2. 查詢功能與定價
3. 觀察範例,看能做到哪些事情
4. 用github搜尋看repository數量評估 



### 常見範例
<ul>
    <li>
        <p><a href="https://jsbin.com/mezifegada/1/edit?js,output" rel="noopener noreferrer" target="_blank">下拉式選單切換地區資料</a></p>
    </li>
    <li>
        <p><a href="http://jsbin.com/gadodidade/1/edit?html,console,output" rel="noopener noreferrer" target="_blank">街景服務</a></p>
    </li>
    <li>
        <p><a href="https://developers.google.com/maps/documentation/javascript/examples/?hl=zh-tw" rel="noopener noreferrer"
                target="_blank">官方範例</a></p>
    </li>
    <li>
        <p><a href="http://maps.vasile.ch/geomask/" rel="noopener noreferrer" target="_blank">geomask</a></p>
    </li>
</ul>

### 1~5 看完, 6~10 =>標記的進階功能未看


## ES6入門 let, const

### 介紹
1. 兼容性可能比較不好
    =>舊瀏覽器可以透過Babel(https://www.udemy.com/projects-in-javascript-jquery/learn/v4/content)來轉換ES6讓舊瀏覽器可以讀取
2. 使用Babel需要搭配其他工具(ex. gulp)

### window var特性
1. 用var a = 1,會讓a變成全域變數
2. for (var i = 0 ; i < 3 ; i++),i也會變成全域變數(i=3)
3. 這樣容易導致程式間互相污染全域變數
4. ES6 - let, const 可以解決這種狀況

### let - if, function用法
1. 直接看範例

    ```js
    // let, const 用來宣告區塊裡的變數
    //區塊 = {}
    if (3 > 2) {
        a = 1;
    }
    // 此a = 全域變數 

    if (3 > 2) {
        let a = 1;
    }
    // 此a != 全域變數


    // 下面做法不會影響到全域變數
    var a = 0;
    function changeA(){
        let a = 0;
        a = 1;
        console.log(a);
    }
    changeA();
    console.log(a);
    ```
    
### let - for用法
1. 看範例

    ```html
    <ul class="list">
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </ul>
    ```

    ```js
    // 不會污染到全域變數(global)
    for (let i = 0 ; i < 3 ; i++){
        console.log(i);
    }

    // 點擊list就alert的list編號
    // 污染到i,結果i都是=3
    const listLen = document.querySelectorAll('.list li').length;
    for (var i = 0 ; i < listLen ; i++){
        document.querySelectorAll('.list li')[i].addEventListener('click',function(){
            alert(i+1);
        }) 
    }

    // 改用let即可
    const listLen = document.querySelectorAll('.list li').length;
    for (let i = 0; i < listLen; i++) {
        document.querySelectorAll('.list li')[i].addEventListener('click', function () {
            alert(i + 1);
        })
    }
    ```

### const
1. const = 唯獨變數,不能被修改
    =>examples: url網址
2. object & array 可以被修改; 如果想要保持object & array不被修改
    =>freeze語法 - Object.freeze();
3. 範例

    ```js
    // 不能被修改, ex. url網址
    // object, array還是可以, 下面範例obj = {'x'}
    const obj ={
        url:'http://www.x.com'
    };
    obj.url = 'x'
    console.log(obj);

    // 如果想要object, array都不能被修改
    const obj ={
        url:'http://www.x.com'
    };
    // 用freeze來凍結
    Object.freeze(obj);
    obj.url = 'x'
    console.log(obj);
    ```

### let, const注意事項與使用時機
1. var會移動到最上面執行, let & const不會

    ```js
    // 1st a = undefined(不是找不到), 2nd a = 3
    console.log(a);
    var a = 3;
    console.log(a);

    // // 改用let(const一樣)
    // // 1st a = 找不到, 2nd a = 3
    console.log(a);
    let a = 3;
    console.log(a);
    ```

2. let, const 同區塊上不能二次命名

    ```js
    var a = 1;
    var a = 2;

    // let, const 這樣寫會顯示b已經被宣告 
    let b = 5;
    let b = 6;
    ```

3. let, const來宣告的話不會進入window(全域變數

## ES6入門 - 字串篇

### 字串相加
1. ES6 - 最外面用``包起來,裡面""中需要用${}來包住
    =>原本與ES6對比,直接看以下範例

    ```js
    const list =document.querySelector('.list');
    const photoUrl =  'https://source.unsplash.com/random/800x600';
    // 原本做法如下
    list.innerHTML = '<li><img src=' + photoUrl + '></li>'
    // ES6
    // 最外面用``包起來,裡面""中需要用${}來包住
    list.innerHTML = `<li><img src="${photoUrl}"></li>`
    ```
    
### 編輯器與小技巧
1. emmet - javascript也能用html上的emmet,並使用縮排
    1. VS code上設定搜尋emmet
    2. 打開setting.json
    3. 六角用這個,我不能用 - 版本問題

    ```js
    "emmet.syntaxProfiles": {"javascript" : "html"}
    ```

    4. 網路上用這個,可用,但是用兩個重音符包起來裡面li>h2+img的要加上第二行

    ```js
    "emmet.includeLanguages": {"javascript": "html"}，
    "emmet.triggerExpansionOnTab": true,
    ```