---
title: "[Vue] - （七）組件-實作"
date: 2019-05-02 18:00:00
tags:
  - Vue
categories:
  - project
---

## 實作-axios 會員登入

此實作有兩個功能，後端server從github上clone下來在local端跑。
一般來說密碼在送出前會先編碼加密過才傳到伺服器上。

- 登入: Signin-Form
- 查詢帳號是否存在: User-Check

![](https://i.imgur.com/xJxkDXv.png)

![](https://i.imgur.com/O3vtNAq.png)


```js
// 後端server資料
{
  username: 'orow',
  password: '1234567',
  name: 'Orow Chou',
}, {
  username: 'taco',
  password: '7654321',
  name: 'Arlik',
}
```

---

**Library**

axios & lodash
ajax - [axios](https://github.com/axios/axios) 

[lodash](https://www.jsdelivr.com/package/npm/lodash)，這邊用到的是debounce

library要先引入cdn

---

**登入**

登入成功
![](https://i.imgur.com/wPWPSUd.png)

輸入錯誤
![](https://i.imgur.com/hc8rStb.png)

發出post request後，回傳的data中有個success會有true或false代表有沒有成功。
如果成功，把回傳的name指回組件data中的loggedUser，並在html中用v-if指到相同的名稱，成功就會顯示字串搭配name。
如果失敗，直接把data中error設為錯誤描述，並在html中v-else顯示出來。

- 呼叫ajax，loading狀態

    submit時要讓兩個input跟button處於不能按的狀態，也就是disable的狀態。
    綁定disabled屬性搭配名稱，在data中給該名稱為false，在submit函式一執行時就改為ture，ajax成功獲取資料後就改為false。

- 錯誤訊息處理

    如果帳號密碼輸入錯誤，顯示錯誤訊息描述，在一修改帳號或者是密碼時就要清空錯誤訊息。
    利用watch偵聽username跟password的雙向綁定來達成清空。
    

**檢查**

Username已存在
![](https://i.imgur.com/SoENQJb.png)

Username不存在
![](https://i.imgur.com/V6w6Gzq.png)


檢查輸入的username是否已經有該資料，data中有雙向綁定的username，還有一個顯示是否存在資料的boolean值。


- 一輸入就偵聽，不需要按按鈕：watch

    一般可以用button來submit，執行方法中的函式。
    但也可以一輸入完就馬上去執行，這邊用了watch來偵聽username，一發生改變就呼叫submit函式。

- 輸入後延遲時間執行：lodash

    username一輸入就會去執行，這邊使用了`lodash`中`debounce`的方法來設定延遲時間，在生命週期的created中就建立this.submitDebounced搭配lodash的debounce，再把watch中的執行函式改為submitDebounced即可。

最後在html中給兩個按鈕可以切換兩個form組件，利用綁定click事件給名稱，在搭配component中`:is`的屬性來顯示，component一開始的屬性要設定在vue實例中的data。


```html
<div id="app">
    <div>
        <button @click="component='signin-form'">Signin-Form</button>
        <button @click="component='user-check'">User-Check</button>
    </div>
    <hr>
    <keep-alive>
        <component :is="component"></component>
    </keep-alive>
</div>

<script>
    const URL = 'http://localhost:3000';

    Vue.component('user-check', {
        data() {
            return {
                username: '',
                exists: false,
            }
        },
        methods: {
            submit() {
                axios.get(`${URL}/exists/${this.username}`)
                    .then(res => {
                        this.exists = res.data.exists;
                    });
            },
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
            <label>Username:</label>
            <input type="text" v-model="username">
            <br>
            <span>{{exists}}</span>
        </div>
        `,
    });

    Vue.component('signin-form', {
        data() {
            return {
                username: '',
                password: '',
                loggedUser: null, // 登入成功會綁定v-if顯示在html上
                error: '', // 登入帳號密碼錯誤的錯誤訊息
                loading: false, // loading會disabled
            }
        },
        methods: {
            submit() {
                this.loading = true;
                axios.post(`${URL}/login`, {
                        username: this.username,
                        password: this.password,
                    })
                    .then(res => {
                        console.log(res.data.success);
                        this.loading = false;
                        if (res.data.success) {
                            this.loggedUser = res.data.name;
                        } else {
                            this.error = 'Incorrect username/password'
                        }
                    });
            },
        },
        // 偵聽一改變帳號或密碼就清除錯誤訊息
        watch: {
            username() {
                this.error = '';
            },
            password() {
                this.error = '';
            },
        },
        template: `
        <div>
            <h1 v-if="loggedUser">Welcome! {{loggedUser}}</h1>
            <form v-else @submit.prevent="submit">
                <label>Username:</label>
                <input type="text" v-model="username" :disabled="loading">
                <br>
                <label>Password:</label>
                <input type="password" v-model="password" :disabled="loading">
                <br>
                <button type="submit" :disabled="loading">Submit</button>
                <br>
                <span v-if="error">{{error}}</span>
            </form>
        </div>
        `,
    })
    new Vue({
        el: '#app',
        data: {
            component: 'signin-form',
        },
    });
</script>
```
