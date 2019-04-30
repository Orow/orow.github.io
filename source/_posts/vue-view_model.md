---
title: "[Vue] - （二）實例View Model"
date: 2019-04-25 15:00:00
tags:
  - Vue
categories:
  - Vue
---

本篇主要是透過線上課程：`HiSKIO`、`w3school`及網路上搜尋資源所學習的。

## 實例 View Model

- 元素掛載（\$mount）、模板（template）
- 狀態（data）
- 方法（methoda）
- 計算屬性（computed）
- 偵聽（watch）
- 生命週期（life cycle hooks）

### 元素掛載、模板

宣告 vue 實例（new Vue）後要連結 html 元素，el 是元素 element 的縮寫，可以使用 CSS selector 的方式也可以用 JS 獲取名稱，要注意不能同時掛載多個相同的元素，只會掛載第一個符合的元素。

元素掛載的部分也可以在宣告實例後，有需要用到再去掛載。

```html
<div id="app">
  <h1>{{msg}}</h1>
</div>

<script>
  // const vm可以省略
  // const vm =  new Vue({
  // });

  const vm = new Vue({
    el: "#app", // mount，CSS selector
    // el: document.getElementById('app'),
    // template是字串，掛載後會取代html中元素裡面的內容
    template: "<div><h2>{{msg}}{{msg}}</h2></div>",
    data: {
      msg: "Hello!!"
    }
  });
  // 宣告實例後，需要再去掛載
  // vm.$mount('#app');
  // vm.$mount(document.getElementById('app'));
</script>
```

### 狀態-data

vue 實例的狀態，或者是說資料，本身是物件。
data 的宣告也可以在實例外面，在實例中再去取用該名稱，也可以重新給值。

```html
<div id="app">{{a+b}}</div>

<script>
  const data = {
    a: 1,
    b: 2
  };
  const vm = new Vue({
    el: "#app",
    // data:{
    //     a:1,
    //     b:2,
    // }
    data // data: data,
  });
  // console.log(vm.a) // 1
  vm.a = 3;
  console.log(data.a); // 3
  console.log(vm.$data === data); // true
</script>
```

### 方法-methods

methods 也是物件，要宣告實例中會用到的 function，在 html 則是要用 v-on:來綁定事件。

也可以在 methods 的其中一個函式用 this.函式名稱去執行另一個 methods 中的函式，但要注意 data 的屬性名稱跟 methods 中函式名稱不能一樣。

在 methods 中不要去使用到箭頭函式，在 methods 中的 this 是指導 vue 實例本身，如果用箭頭函式 this 則會指到 window。

```html
<div id="app">
  <h1>{{count}}</h1>
  <button v-on:click="add1">+1</button>
  <br />
  <button v-on:click="add2">+{{step}}</button>
  <br />
  <button v-on:click="add3">+{{step}}</button>
</div>

<script>
  const vm = new Vue({
    el: "#app",
    // data中的屬性跟methods中的函式不能一樣
    data: {
      count: 0,
      step: 1
    },
    // methods中函式不能用箭頭函式，this不會指到實例會指到window
    methods: {
      add1() {
        this.count += 1;
      },
      add2() {
        this.count += this.step;
        this.step += 1;
      },
      add3() {
        this.add2();
      }
    }
  });
</script>
```

### 計算屬性-computed

computed 也是物件，裡面可以宣告成函式，也可以宣告為物件。
物件中可以宣告 get、set 函式，在取用這個物件的時候，程式就會直接執行 get 這個函式。當要改變物件名稱的值的時候就會去執行 set 函式。
宣告函式與宣告物件中的 get 都需要 return。

用 input 相加的結果來記錄說明
![](https://i.imgur.com/NOZCVEb.png)

```html
<div id="app">
  <input type="number" v-model="a" />
  +
  <input type="number" v-model="b" />
  =
  <!-- <span>{{answer}}</span> -->
  <input type="number" v-model="answer" />
  <br />
  {{a}}+{{b}}={{answer}}
</div>

<script>
  const vm = new Vue({
    el: "#app",
    data: {
      a: 0,
      b: 0
    },
    // computed裡可以宣告成函式，也可以宣告為物件
    computed: {
      // answer(){
      //     return parseInt(this.a) + parseInt(this.b);
      // }

      // 物件中可以宣告get, set函式，要取用answer值就會呼叫get函式，要設定時就呼叫set函式
      answer: {
        get() {
          return parseInt(this.a) + parseInt(this.b);
        },
        // set 不需要return，val參數就是去設定的值
        set(val) {
          this.b = parseInt(val) - parseInt(this.a);
        }
      }
    }
  });
</script>
```

### 偵聽-watch

watch 顧名思義就是要監聽，型態同樣是物件，裡面可以宣告函式，也可以宣告成物件。
宣告的名稱需要跟 data 或是 computed 裡面的屬性名稱是相同的。

宣告為函式時，()中第一個參數是現在的值，第二個參數是上一個舊的值。

```html
<div id="app">
  <input type="text" v-model="value" />
  <span>{{valuex2}}</span>
</div>

<script>
  const vm = new Vue({
    el: "#app",
    data: {
      value: 0
    },
    methods: {},
    computed: {
      valuex2() {
        return parseInt(this.value) * 2;
      }
    },
    // 物件，宣告成函式，命名要跟data一樣，會偵聽data, methods(不會變), computed中該屬性的值
    watch: {
      value(val, oldVal) {
        console.log(`${oldVal} -> ${val}`);
      },
      valuex2(val, oldVal) {
        console.log(`${oldVal} -> ${val}`);
      }
    }
  });
</script>
```

宣告為物件時，裡面要有一個 handler 函式，還有兩個屬性

- immediate: ture or false
- deep: ture or false

`immediate`為一初始化就跑一次 watch
`deep`偵聽對象只能是「屬性的內容」為陣列或是物件

不能濫用 watch

像這個案例 input - a+b=c
a 與 b 在 watch 要分開偵聽，實效能差，且不符合聲明，實務上用 computed 直接偵聽 c 即可。

```html
<div id="app">
  <!-- deep -->
  <!-- <input type="text" v-model="input.a"> -->

  <!-- watch 命令式 -->
  <input type="text" v-model="a" />+ <input type="text" v-model="b" />=
  <input type="text" v-model="c" />
</div>

<script>
  const vm = new Vue({
    el: "#app",
    data: {
      // input:{
      //     a:"",
      // },
      a: 0,
      b: 0,
      c: 0
    },
    methods: {},
    computed: {
      // 用computed較簡潔，效能較好
      c() {
        return parseInt(this.a) + parseInt(this.b);
      }
    },
    watch: {
      // value:{
      //     handler(val, oldVal){
      //         console.log(`${oldVal} -> ${val}`);
      //     },
      // input:{
      //     handler(val, oldVal){
      //         console.log(val === oldVal);
      //         console.log(`${oldVal} -> ${val}`);
      //     },
      //     // true, 初始化就會跑一次watch
      //     // immediate: true,

      //     // deep只能用來偵聽「屬性的內容」是陣列或是物件
      //     deep: true,
      // },

      // 不符合聲明式且效能差，直接用computed，比較簡潔也符合聲明式，效能也比較好
      a(val) {
        this.c = parseInt(val) + parseInt(this.b);
      },
      b(val) {
        this.c = parseInt(val) + parseInt(this.a);
      }
    }
  });
</script>
```

### 生命週期

- beforeCreate
  server side rendering
  創立實例，一開始初始化後就執行

- create
  server side rendering
  data、computed 等屬性放到實例裡面

以下六種都在 client side rendering

- beforeMount
  當決定 render 的時候會執行，掛載修改的內容（模板）

- Mounted
  掛載後才執行這個函式
  最常用的，ajax 或偵聽事件元素或 window 等等

- beforeUpdate
  資料變更先跑這個

- updated
  資料變更後會跑 updated 函式

- beforeDestroy
  呼叫 vm.\$destroy 摧毀前會先執行

- destroyed
  呼叫 vm.\$destroy，全部 vue 摧毀
  取消偵聽等等

![](https://i.imgur.com/GuRQpSr.png)
![](https://i.imgur.com/z3mwPfj.png)
![](https://i.imgur.com/WxdvMl0.png)

## 實作-密碼強度檢查

會員註冊時，即時的檢查，如密碼太短，只用數字不夠強等等。

- v-model雙向綁定data
- computed計算密碼強度（長度），包含用正規表達式判斷大小寫數字等等。

```html
<div id="app">
  <input type="password" v-model="password" />
  <p>{{strength}}</p>
</div>

<script>
  const vm = new Vue({
    el: "#app",
    data: {
      password: ""
    },
    computed: {
      strength() {
        let score = this.password.length;
        // regular expression
        if (/[A-Z]/.test(this.password)) score *= 1.25;
        if (/[a-z]/.test(this.password)) score *= 1.25;
        if (/[0-9]/.test(this.password)) score *= 1.25;
        if (/[^A-Za-z0-9]/.test(this.password)) score *= 1.25;

        if (score > 40) return "Perfect";
        if (score > 20) return "Great";
        if (score > 10) return "Good";
        return "Weak";
      }
    }
  });
</script>
```
