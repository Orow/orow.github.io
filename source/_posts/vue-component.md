---
title: "[Vue] - （七）組件"
date: 2019-05-02 15:00:00
tags:
  - Vue
categories:
  - Vue
---

## 組件 Component

### Vue 組件的註冊與使用

可以重複使用，開發時可以縮小焦點，容易維護。
使用 vue 組件，視覺元素(html)是用 option 中的 template 來表示。

宣告順序：`vue component的宣告要在vue instance以前`

全域（Global）：Vue.component('名稱', {optiopn，template:'...'等})
component 名字最好是全小寫，用 dash 分開。

Local：在 vue 實例中宣告，只會存在這個實例中。

```html
<div id="app">
  <my-component></my-component>
  <local-component></local-component>
</div>

<script>
  // Global
  Vue.component("my-component", {
    // option
    template: "<div>My component</div>" // 定義視覺元素(html)
  });
  new Vue({
    el: "#app",
    // Local
    components: {
      "local-component": {
        template: "<div>Local component</div>"
      }
    }
  });
</script>
```

### DOM 模板的解析注意事項

如果在 vue 實例中用掛載 el 的方式來指定綁定的 html 元素，這個元素就會經過 html 瀏覽器的解析再把元素傳給 vue 實例來使用，如果在解析過程中瀏覽器發現不符合格式或不合法的宣告就會把它移除，之後在 vue 實例中就會沒辦法使用。

select 中不應該有 my-component。

除了 select 以外，還有 ul、table 中也是（chrome 可以）。

```html
<div id="app">
  <select>
    <my-component></my-component>
  </select>
  <ul>
    <ul-component></ul-component>
  </ul>
  <table>
    <table-component></table-component>
  </table>
</div>

<script>
  Vue.component("my-component", {
    template: "<option>Hello</option>"
  });
  Vue.component("ul-component", {
    template: "<li>Hello</li>"
  });
  Vue.component("table-component", {
    template: "<tr><td>Hello</tr></td>"
  });
  new Vue({
    el: "#app"
  });
</script>
```

如果遇到這個問題，把 el 搭配 template 來使用即可。

```html
<div id="app"></div>

<script>
  Vue.component("my-component", {
    template: "<option>Hello</option>"
  });
  new Vue({
    el: "#app",
    template:
      '<div id="app"><select><my-component></my-component></select></div>'
  });
</script>
```

### data 必須是函數

組件的 data（函數）與實例的 data（物件）不同

每次執行 data 都會執行，如果有兩個一樣的組件要重複使用，會是分開的執行，不會互相影響。
如果是宣告成物件，一開始就會宣告，但重複使用不會再去宣告。

```html
<div id="app">
  <!-- <h1>{{count}}</h1>
    <button @click="count+=1">+1</button> -->
  <counter></counter>
  <counter></counter>
</div>

<script>
  Vue.component("counter", {
    // 每次執行data都會執行，如果有兩個一樣的組件要重複使用，會是分開的執行，不會互相影響
    // 如果是宣告成物件，一開始就會宣告，但重複使用不會再去宣告
    data() {
      return {
        count: 0
      };
    },
    template: `
        <div>
            <h1>{{count}}</h1>
            <button @click="count+=1">+1</button>
        </div>
        `
  });

  new Vue({
    el: "#app"
    // data:{
    //     count: 0,
    // },
  });
</script>
```

### 組件組合

用簡單的 todolist 來記錄說明，這個例子宣告了三個組件 todolist，todo-input，todo-item。

上層傳到下層：`props`
最上層的 todolist，todo-input，todo-item，要把 todolist 中的 todos 陣列顯示在 todo-item 裡面，在上層需要透過`屬性綁定`的方式，下層則是需要用`props`來帶入屬性名稱。

下層傳到上層：`$emit`
下層的 todo-input 要傳資料到上層的 todolist，透過 form 綁定 submit 事件來執行發送`事件`的函式。上層則是需要綁定下層要發送的事件名稱後去執行新增內容的函式。

```html
<div id="app">
  <todolist></todolist>
</div>

<script>
  Vue.component("todo-input", {
    data() {
      return {
        text: ""
      };
    },
    methods: {
      submit() {
        // 第一個參數：發出input事件，第二個參數：input事件帶的是this.text的值
        this.$emit("input", this.text);
        this.text = "";
      }
    },
    template: `
            <form @submit.prevent="submit">
                <input type="text" v-model="text">
                <button type="submit">Submit</button>
            </form>
        `
  });
  Vue.component("todo-item", {
    // 宣告props名稱
    props: ["label"],
    template: `<li>{{label}}</li>`
  });
  Vue.component("todolist", {
    data() {
      return {
        todos: ["a", "b", "c"]
      };
    },
    methods: {
      // 下層input事件emit後，要執行此function
      addTodo(text) {
        this.todos.push(text);
      }
    },
    template: `
        <div>
            <todo-input @input="addTodo"></todo-input>
            <ul>
                <todo-item v-for="todo in todos" :label="todo"></todo-item>
            </ul>
        </div>    
        `
  });
  new Vue({
    el: "#app"
  });
</script>
```

### 動態組件

兩個組件可以透過動態的方式來切換，透過`:is`綁定 is 屬性搭配 data 中相同的key，來給屬性名稱。
透過點擊事件綁定不同的名稱來切換不同組件。

基本 SPA 運用

```html
<div id="app">
  <!-- 點擊會變更data中的content -->
  <button @click="content='lessons'">Lessons</button>
  <button @click="content='apply'">Apply</button>
  <br />
  <!-- :is 來自data中的content -->
  <component :is="content"></component>
</div>

<script>
  Vue.component("apply", {
    template: `
        <form>
            <textarea></textarea>
            <br>
            <button>submit</button>
        </form>
        `
  });
  Vue.component("lessons", {
    template: `
        <ul>
            <li>Vue tutorial</li>
            <li>React tutorial</li>
            <li>Angular tutorial</li>
        </ul>
        `
  });
  new Vue({
    el: "#app",
    data: {
      content: "lessons"
    }
  });
</script>
```

### 動態組件 keep alive

延續上面的動態組件

在切換的時候原本組件輸入的資料會被消滅再重建，如果想要保留，可以使用`keep-alive`的 tag 包住，即可以保存下來。

```html
<!-- keep-alive 切換組件要保留原本的資料 -->
<keep-alive>
  <component :is="content"></component>
</keep-alive>
```

### 組件命名

組件中名稱的命名可以使用三種

- kebab-case: `my-first-component`，dash 隔開
- camel-case: `myFirstComponent`
- pascal-case: `MyFirstComponent`

但是在 html 中只能用`kebab-case`，組件三種命名方式，使用的時候都會轉為 kebab-case，在 html 去執行。

以上限制只有在用 el 綁定元素，如果用 template 模板綁定則沒有限制。

```html
<div id="app">
  <!-- html中只能用kebab-case -->
  <my-first-component></my-first-component>
</div>

<script>
  // my first component
  "my-first-component"; // kebab-case - html中只能用這個
  "myFirstComponent"; // camel-case - template中皆可
  "MyFirstComponent"; // Pascal-case

  Vue.component("my-first-component", {
    template: "<h1>Hello</h1>"
  });

  new Vue({
    // el 綁定html則在html只能用kebab case
    el: "#app"

    // template不受限制
    // template:`
    // <div>
    //     <myFirstComponent></myFirstComponent>
    //     self-closing
    //     <myFirstComponent>
    // </div>
    // `,
  });
</script>
```

## 實作-axios 會員登入

此實作有兩個功能，後端 server 從 github 上 clone 下來在 local 端跑。
一般來說密碼在送出前會先編碼加密過才傳到伺服器上。

- 登入
- 查詢帳號是否存在

---

**Library**

axios & lodash
[axios](https://github.com/axios/axios) - ajax

[lodash](https://www.jsdelivr.com/package/npm/lodash)，這邊用到的是 debounce

library 要先引入 cdn

---

**登入**

發出 post request 後，回傳的 data 中有 success 會帶有 true 或 false 代表有沒有成功。
如果成功，把回傳的 name 指回組件 data 中的 loggedUser，並在 html 中用 v-if 指到相同的名稱，成功就會顯示字串搭配 name。
如果失敗，直接把 data 中 error 設為錯誤描述，並在 html 中 v-else 顯示出來。

- 呼叫 ajax，loading 狀態

  submit 時要讓兩個 input 跟 button 處於不能按的狀態，也就是 disabled 的狀態。
  綁定 disabled 屬性搭配名稱，在 data 中給該名稱為 false，在 submit 函式一執行時就改為 ture，ajax 成功獲取資料後就改為 false。

- 錯誤訊息處理

  如果帳號密碼輸入錯誤，顯示錯誤訊息描述，在一修改帳號或者是密碼時就要清空錯誤訊息。
  利用 watch 偵聽 username 跟 password 的雙向綁定來達成清空。

**檢查**

檢查輸入的 username 是否已經有該資料，data 中有雙向綁定的 username，還有一個顯示是否存在資料的 boolean 值。

- 一輸入就偵聽，不需要按按鈕：watch

  一般可以用 button 來 submit，執行方法中的函式。
  但也可以一輸入完就馬上去執行，這邊用了 watch 來偵聽 username，一發生改變就呼叫 submit 函式。

- 輸入後延遲時間執行：lodash

  username 一輸入就會去執行，這邊使用了`lodash`中`debounce`的方法來設定延遲時間，在生命週期的 created 中就建立 this.submitDebounced 搭配 lodash 的 debounce，再把 watch 中的執行函式改為 submitDebounced 即可。

最後在 html 中給兩個按鈕可以切換兩個 form 組件，利用綁定 click 事件給名稱，在搭配 component 中`:is`的屬性來顯示，component 一開始的屬性要設定在 vue 實例中的 data。

```html
<div id="app">
  <div>
    <button @click="component='signin-form'">Signin-Form</button>
    <button @click="component='user-check'">User-Check</button>
  </div>
  <hr />
  <keep-alive>
    <component :is="component"></component>
  </keep-alive>
</div>

<script>
  const URL = "http://localhost:3000";

  Vue.component("user-check", {
    data() {
      return {
        username: "",
        exists: false
      };
    },
    methods: {
      submit() {
        axios.get(`${URL}/exists/${this.username}`).then(res => {
          this.exists = res.data.exists;
        });
      }
    },
    watch: {
      username() {
        // this.submit();
        this.submitDebounced();
      }
    },
    created() {
      // lodash library中的debounce來實現delay
      // 執行為submitDebounced後，經過設定的時間後才去執行this.submit，過程中不會重複執行
      // 第一個參數為某一個函式，第二個為延遲時間ms
      this.submitDebounced = _.debounce(this.submit, 500);
    },
    template: `
        <div>
            <label>Username</label>
            <input type="text" v-model="username">
            <br>
            <span>{{exists}}</span>
        </div>
        `
  });

  Vue.component("signin-form", {
    data() {
      return {
        username: "",
        password: "",
        loggedUser: null, // 登入成功會綁定v-if顯示在html上
        error: "", // 登入帳號密碼錯誤的錯誤訊息
        loading: false // loading會disabled
      };
    },
    methods: {
      submit() {
        this.loading = true;
        axios
          .post(`${URL}/login`, {
            username: this.username,
            password: this.password
          })
          .then(res => {
            console.log(res.data.success);
            this.loading = false;
            if (res.data.success) {
              this.loggedUser = res.data.name;
            } else {
              this.error = "Incorrect username/password";
            }
          });
      }
    },
    // 偵聽一改變帳號或密碼就清除錯誤訊息
    watch: {
      username() {
        this.error = "";
      },
      password() {
        this.error = "";
      }
    },
    template: `
        <div>
            <h1 v-if="loggedUser">Welcome! {{loggedUser}}</h1>
            <form v-else @submit.prevent="submit">
                <label>Username</label>
                <input type="text" v-model="username" :disabled="loading">
                <br>
                <label>Password</label>
                <input type="password" v-model="password" :disabled="loading">
                <br>
                <button type="submit" :disabled="loading">Submit</button>
                <br>
                <span v-if="error">{{error}}</span>
            </form>
        </div>
        `
  });
  new Vue({
    el: "#app",
    data: {
      component: "signin-form"
    }
  });
</script>
```

## 影音不同步-promise_callback

`https://randomuser.me/api` 使用者的假資料，可以用來 api 測試

---

**callback**

透過 jQuery 的`$.ajax`或是`$.getJSON`，發出 request 到指定的 url 後，都會有 callback 的 function。
這是在非同步呼叫完成後，才會去執行。

ajax 非同步的呼叫，不知道什麼時候才會完成，如果非同步跟同步的呼叫寫在一起，很容易會有 undefined 的結果發生。

要用 callback function 回傳才能確保值的回傳。

```js
// callback
// jQuery
// function getRandomEmail() {
//     let email;
//     $.getJSON('https://randomuser.me/api', function (data) {
//         email = data.results[0].email;
//     });
//     return email; // 會比ajax非同步呼叫還早執行
// }
// const email = getRandomEmail();
// console.log(email); //undefined

// getRandomEmail函式加上callback，會從ajax完成後的callback傳入
function getRandomEmail(callback) {
  $.getJSON("https://randomuser.me/api", function(data) {
    callback(data.results[0].email); // ajax完成後的callback
  });
}
getRandomEmail(function(email) {
  console.log(email);
});
```

如果要連續獲取多個 email 呢？延續上面例子，就需要用巢狀的方式來呼叫 callback 函式，可讀性就會很低，所謂的 callback hell 回呼地獄。

```js
// 多個
const emails = [];
getRandomEmail(function(email) {
  emails.push(email);
  getRandomEmail(function(email) {
    emails.push(email);
    getRandomEmail(function(email) {
      emails.push(email);
      console.log(emails);
    });
  });
});
```

這時候就可以使用 promise 的方式。

**promise**

promise 是物件，有很多種狀態，執行中，執行完，執行失敗等等，每一個 promise 間可以用`.then()`連結起來。

`.then()`中的 return 是要給接著的.then 使用。

```js
function getRandomEmailPromise() {
  // new Promise函式括號中兩個參數都是函式
  return new Promise(function(resolve, reject) {
    $.getJSON("https://randomuser.me/api", function(data) {
      resolve(data.results[0].email); // ajax呼叫成功就會呼叫resolve
      // 失敗就是reject
    });
  });
}
const emailsPromise = [];
getRandomEmailPromise()
  .then(function(email) {
    emailsPromise.push(email);
    return getRandomEmailPromise(); // return後給接下來的.then使用
  })
  .then(function(email) {
    emailsPromise.push(email);
    return getRandomEmailPromise(); // return後給接下來的.then使用
  })
  .then(function(email) {
    emailsPromise.push(email);
    console.log(emailsPromise);
  });
```

async / await
在 ES6~ES8 中，promise 可以變成很像同步呼叫一樣去使用

```js
// Promise
function getRandomEmailPromise() {
  // new Promise函式括號中兩個參數都是函式
  return new Promise(function(resolve, reject) {
    $.getJSON("https://randomuser.me/api", function(data) {
      resolve(data.results[0].email); // ajax呼叫成功就會呼叫resolve
      // 失敗就是reject
    });
  });
}
const emailsPromise = [];

// async /await ES6~ES8，promise變成很像同步呼叫一樣去使用
async function getEmails() {
  // let email;
  // email = await getRandomEmailPromise();
  // emailsPromise.push(email);
  // email = await getRandomEmailPromise();
  // emailsPromise.push(email);
  // email = await getRandomEmailPromise();
  // emailsPromise.push(email);

  // ES8，瀏覽器支援度目前不夠高
  emailsPromise.push(await getRandomEmailPromise());
  emailsPromise.push(await getRandomEmailPromise());
  emailsPromise.push(await getRandomEmailPromise());
  console.log(emailsPromise);
}
getEmails();
```
