---
title: "[Vue] - （五）事件處理 Event handling"
date: 2019-04-30 15:00:00
tags:
  - Vue
categories:
  - Vue
---

本篇主要是透過線上課程：`HiSKIO`、[官方文件](https://vuejs.org/v2/guide/single-file-components.html)及網路上搜尋資源所學習的。

## 事件處理 Event handling

### 處理事件的兩種方式

- v-on

1. 綁定事件，再給予名稱，之後在實例中宣吿 methods 中該名稱的 function。
2. 在 v-on 後綁定事件後，給予的名稱可以直接寫入表達式，就不需要在 methods 中宣告函式。

```html
<div id="app">
  <h1>{{count}}</h1>
  <!-- <button @click="add">+1</button> -->

  <button @click="count+=1">+1</button>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      count: 0
    }

    // methods: {
    //     add() {
    //         this.count += 1;
    //     },
    // },
  });
</script>
```

### 偵聽滑鼠事件

- mouseover, mouseout: 移進去，移出來。
- mousedown, mouseup: 點擊下去，點擊後。
- mousemove: 在裡面一有移動就會觸發。

```html
<div id="app">
  <h1>{{count}}</h1>
  <!-- 移進去，移出來 -->
  <button @mouseover="count+=1" @mouseout="count-=1">
    Mouse Move in & out
  </button>
  <br />
  <!-- 點擊下去，點擊後 -->
  <button @mousedown="count+=1" @mouseup="count-=1">Mouse Click</button>
  <br />
  <!-- 在裡面一有移動 -->
  <button @mousemove="count+=1">Mouse Move</button>
</div>
<script>
  new Vue({
    el: "#app",
    data: {
      count: 0
    }
  });
</script>
```

### 偵聽圖片的 load 事件

在讀取圖片的時候顯示 Loading 文字，讀取完成則顯示 Complete 文字、圖片顯示出來並帶有效果。
![](https://i.imgur.com/SkEggov.png)

v-if 搭配 v-else 顯示文字，img 元素則綁定 load 事件，屬性綁定一個帶有效果的 class 名稱。
在 computed 判斷 loading 是 true or false 來給予 class 名稱。

```html
<style>
  .img.hide {
    opacity: 0;
    transform: scale(3) rotate(180deg);
  }
  .img {
    transition: all 1s;
  }
</style>

<div id="app">
  <h4 v-if="loading">Loading</h4>
  <h4 v-else>Complete</h4>
  <img
    @load="loading=false"
    :class="imgClass"
    src="https://source.unsplash.com/random/320x240"
  />
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      loading: true
    },
    computed: {
      imgClass() {
        if (this.loading) {
          return "img hide";
        } else {
          return "img";
        }
      }
    }
  });
</script>
```

### 偵聽按鍵事件

todolist 的例子，輸入內容後按下 enter，把輸入的內容加到清單中。

input 用 v-model 雙向綁定，搭配@keydown。清單用 ul 搭配 v-for 來顯示。

偵聽事件的時候是給予一個名字的時候，函式會有一個參數，在事件發生的時候，程式會回傳參數到函式中，按鍵可以使用參數.keyCode。可以 console 出來確認 keyCode 號碼。

```html
<div id="app">
  <input v-model="input" @keydown="keydown" />
  <ul>
    <li v-for="item in lists">{{item}}</li>
  </ul>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      input: "",
      lists: []
    },
    methods: {
      // 事件發生時，程式會把事件回傳函式參數中
      keydown(event) {
        // console.log(evt.keyCode);
        // keyCode of enter is 13
        if (event.keyCode === 13) {
          this.lists.push(this.input);
          this.input = "";
        }
      }
    }
  });
</script>
```

Vue 可以使用修飾符，就不需要去 console keyCode 去查詢。

- enter 就用 @keydown.enter
- a 按鍵就用 @keydown.a，但是這樣還是會產生 a 的文字，所以可以再加上修飾符 prevent 來預防產生預設行為。@keydown.a.prevent

```html
<div id="app">
  <!-- keydown.enter 不需要找key.Code -->
  <input v-model="input" @keydown.enter="enter" />

  <!-- keydown.a 要預防a也出現，可以加上修飾符prevent -->
  <input v-model="input" @keydown.a.prevent="a" />
  <ul>
    <li v-for="item in lists">{{item}}</li>
  </ul>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      input: "",
      lists: []
    },
    methods: {
      enter() {
        this.lists.push(this.input);
        this.input = "";
      },
      a() {
        this.lists.push(this.input);
        this.input = "";
      }
    }
  });
</script>
```

### 修飾符語法糖

語法糖是指可以用很簡短的方式來達成原先要花費一些時間才能達成的功能。

[官方 v-on 修飾符](https://vuejs.org/v2/api/#v-on)
![](https://i.imgur.com/K133RQI.png)

- keyCode
- prevent
- stop
- self
- once

@keydown.enter: v-on 綁定 keydown 事件，用修飾符.enter 即可以偵聽 enter 按鍵 keydown。也可以直接用 keyCode，@keydown.13也是相同結果。

```html
<div id="app">
  <!-- 兩者相同 -->
  <input @keydown.enter="enter" />
  <input @keydown.13="enter" />
</div>
```

`.prevent`則是`event.preveDefault()`的效果。

```html
<div id="app">
  <a href="https://www.google.com/" target="_blank" @click="Click">google</a>
  <br />
  <a href="https://www.google.com/" target="_blank" @click.prevent="linkClick"
    >google-syntactic-sugar</a
  >
</div>

<script>
  new Vue({
    el: "#app",
    data: {},
    methods: {
      Click(evt) {
        evt.preventDefault();
        alert("Don't");
      },
      linkClick() {
        alert("syntactic-sugar-Don't");
      }
    }
  });
</script>
```

`.stop`則是`event.stopPropagation()`的效果。
這個案例是停止事件冒泡（bubbling），如果沒有停止點擊那一個都會 bubbling 到最外層。

```html
<div id="app">
  <!-- 3 bubbling-->
  <div class="box" @click="msg='C'">
    <div class="box" @click="msg='B'">
      <!-- <div class="box" @click="setA"> -->
      <div class="box" @click.stop="msg='A'"></div>
    </div>
  </div>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      msg: ""
    },
    methods: {
      setA(evt) {
        evt.stopPropagation();
        this.msg = "A";
      }
    }
  });
</script>
```

`.self`是觸發事件是自己本體才會執行該事件，此案例點擊到「自己本體」才會帶入 msg。

```html
<div id="app">
  <div class="box" @click.self="msg='C'">
    <div class="box" @click.self="msg='B'">
      <div class="box" @click.self="msg='A'"></div>
    </div>
  </div>
  <h1>{{msg}}</h1>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      msg: ""
    }
  });
</script>
```

`once`事件只會執行一次，此案例為點擊後，只有第一次 count 會+1。

```html
<div id="app">
  <h1>{{count}}</h1>
  <button @click="count+=1">+1</button>
  <button @click.once="count+=1">+1</button>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      // 5
      count: 0
    }
  });
</script>
```
