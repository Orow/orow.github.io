---
title: "[Vue] - （一）基本概念"
date: 2019-04-24 15:00:00
tags:
  - Vue
categories:
  - Vue
---

本篇主要是透過線上課程：`HiSKIO`、`w3school`及網路上搜尋資源所學習的。

Vue.js 是一個提供 MVVM 風格的雙向數據綁定的 Javascript 庫，專注於View 層。它的核心是 MVVM 中的 VM，也就是 ViewModel。ViewModel負責連接 View 和 Model，保證視圖和數據的一致性，這種輕量級的架構讓前端開發更加高效、便捷。

原文網址：https://kknews.cc/zh-tw/tech/jv8ykye.html

## 基本概念

MVVM
![](https://i.imgur.com/r3E7wCI.png)

Vue 分三部分

- Vue 模板
- Vue 實例
- Vue 組件

會模板跟實例就可以開始嘗試開發
![](https://i.imgur.com/qTXr1yE.png)

主流框架：Vue, React, Angular 都是聲明式渲染
jQuery 是命令式，一口令一動作

### if else:流程控制與迴圈

- v-if
- v-else
- v-for

v-if 與 v-else 可以搭配使用，如果 v-if 綁定的 see 是 false 則會執行接在下面的 v-else。
v-for 則是需要注意命名方式，如 step in steps，後面的 steps 需要宣告在 data 中。

```html
<div id="app">
  <!-- 基本 -->
  <span>{{message}}</span>

  <!-- if else -->
  <span v-if="see">Now you see me!</span>
  <span v-else="see">Now you don't.</span>

  <!-- for -->
  <div v-for="step in steps">{{step}}</div>
</div>
```

```js
// 基本
new Vue({
  el: "#app",
  data: {
    message: "Hello Orow, this is new fWorld"
  }
});

// if-else
new Vue({
  el: "#app",
  data: {
    see: false
  }
});

// for
new Vue({
  el: "#app",
  data: {
    steps: ["學 JavaScript", "學 Vue", "更多技術"]
  }
});
```

### 處理使用者輸入

- v-on
- v-model

v-on 可以綁定 method 名稱，當然也需要在實例中 methods 中宣告相同名稱的 function。
v-model 則是在 vue 中用來實現雙向綁定的方式

```html
<div id="counter">
  <!-- add +1 v-on監聽-->
  <h1>{{count}}</h1>
  <button v-on:click="add">+1</button>

  <!-- remove -1 v-on監聽 -->
  <ul>
    <li v-for="item in items">{{item}}</li>
  </ul>
  <button v-on:click="remove">-1</button>

  <!-- 雙向綁定 v-model -->
  <input type="text" v-model="text" />
  <span>{{text}}</span>
</div>
```

```js
// add +1 v-on監聽
new Vue({
  el: "#counter",
  data: {
    count: 0
  },
  methods: {
    add() {
      this.count += 1;
    }
  }
});

// remove -1 v-on監聽
new Vue({
  el: "#counter",
  data: {
    items: [1, 2, 3, 4, 5]
  },
  methods: {
    remove() {
      this.items.pop();
    }
  }
});

// 雙向綁定 v-model
new Vue({
  el: "#counter",
  data: {
    text: ""
  }
});
```

## 模板語法 template syntax

### 雙大括號語法，插入動態的數值或文字

- v-once :只會 render 一次（第一次)
- v-html :可以動態加入 html tag

v-html 會有安全性的疑慮，XSS(cross sitr script)

```html
<div id="app">
  <!-- V-once 只會渲染一開始的參數 -->
  <span v-once>{{message}}</span>
  <br />
  <span>{{message}}</span>
  <br />
  <button v-on:click="append">Append</button>

  <!-- v-html 插入html tag -->
  <span v-html="name"></span>
</div>
```

```js
// v-once
new Vue({
  el: "#app",
  data: {
    message: "Hello world!"
  },
  methods: {
    append() {
      this.message += "!";
    }
  }
});
// v-html
new Vue({
  el: "#app",
  data: {
    name: "<h1>Hello World!</h1>"
  }
});
```

### v-bind, v-on

- v-bind: 屬性修改
- v-on: 監聽 DOM 事件來改變資料

縮寫

- v-bind: `:`
- v-on: `@`

屬性的資料除了直接指定變數以外也可以塞入表達式（expression）-下面例子是偶數的部分才會 checked

```html
<div id="app">
  <!-- 如果要塞值的地方不是DOM，而是內部屬性 -->
  <input type="checkbox" v-bind:checked="selected" />
  <button v-on:click="toggle">Toggle</button>

  <!-- 屬性的資料塞入表達式 -->
  <span>{{count}}</span>
  <input type="checkbox" v-bind:checked="count%2===0" />
  <button v-on:click="add">Add</button>
</div>
```

```js
// 如果要塞值的地方不是DOM，而是內部屬性
new Vue({
  el: "#app",
  data: {
    selected: false
  },
  methods: {
    toggle() {
      this.selected = !this.selected;
    }
  }
});

// 屬性的資料塞入表達式
new Vue({
  el: "#app",
  data: {
    count: 0
  },
  methods: {
    add() {
      this.count += 1;
    }
  }
});
```

### 雙向綁定 v-model

輸入 input 也同時改變動態的文字

```html
<div id="app">
  <input type="text" v-model="text" />
  <br />
  {{text}}
</div>
<script>
  new Vue({
    el: "#app",
    data: {
      text: "Hello"
    }
  });
</script>
```
