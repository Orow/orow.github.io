---
title: "[Vue] - （十）單一檔案組件 & Vue-cli"
date: 2019-08-30 15:00:00
tags:
  - Vue
categories:
  - Vue
---

本篇主要是透過線上課程：`HiSKIO`、[官方文件](https://vuejs.org/v2/guide/single-file-components.html)及網路上搜尋資源所學習的。

## 單一檔案組件（single file components）

在許多 Vue 項目中，使用`Vue.component`定義全域組件，然後再用`new Vue({ el: '#app' })`，在要使用的頁面中去指定到該容器元素。
這樣的方式在一些中小型專案沒有什麼大問題，這些專案中 JavaScript 被用來加強特定的 views ，但在更複雜的大型專案，或是前端都用 JavaScript 來驅動的時候，就會把一些缺點放大。

- 全域定義(Global definitions)：強制每個組件 component 的命名不能重複。
- 字串模板(String templates)：元件中的 template 屬性只能用字串，在編輯器中編寫內容時，無法有效地以顏色區分，而且只能用`\`反斜線的方式來進行斷行。
- 不支援CSS(No CSS support)：在組件化時，元件只能提供 HTML 及 JavaScript，無法支援 CSS 。
- 沒有前置處理(No build step)：不能使用預處理工具，ex: Babel，預設限制只能使用 HTML 及 ES5 。

用`單一檔案元件(single file components)`來開發 vue ，包含 HTML、Script、CSS 等等都會放在同一個檔案，一個檔案就代表一個組件，為了這個方式來開發，需要用工具來轉換打包。
需要先安裝 node.js ， npm 安裝套件等等，其中用來打包轉換的 webpack 的工具。

Webpack 在自己建置時相對會比較複雜， vue-cli 則是可以比較簡單的建立一個模板來直接使用，簡單的說就是 vue-cli 讓使用者用更簡易的方式來建立模板。

**單一檔案組件實例**

![](https://i.imgur.com/kgR8Zw3.png)

### import & export

大型專案或開發到後來，用一個檔案很難維護，現代的前端開發習慣拆成很多個檔案， vue 的開發一個檔案就會代表一個組件，透過 export 跟 import 來使用。

export - Counter.js

```js
const Counter = Vue.component('counter',{
    // ....
}); 

export default Counter;
```

import - App.js

```js
import Counter from './Counter' // 如果檔名是.js可以省略

new Vue({
    el: '#app',
    components: {
        Counter,
    },
    template:`
    <div>
        <Counter/>
    </div>
    `
})
```

### Vue-cli 建立模板

- 在 terminal 輸入`vue init webpack-simple 專案資料夾名稱`
- 接著會有專案名稱、描述、作者等等選項
- 完成後需要按照步驟來 npm install，npm run dev 等

![](https://i.imgur.com/6WoVwoF.png)

npm run dev 後瀏覽器則會自己開啟`http://localhost:8080/`的位址
![](https://i.imgur.com/Mjp9VIk.png)

回到專案資料夾中 src 裡面的 main.js 

```js
import Vue from 'vue'
import App from './App.vue'

new Vue({
  el: '#app',
  render: h => h(App)
})
```

定義 vue 實例在畫面上呈現什麼東西，第一個是透過 html 直接定義模板，然後把 el 指向該元素，第二個則是 vue 實例中用`template`，第三個也是在 vue 實例中用`render`。

原本的 render 寫法，本身是函式，會傳入一個 createElement 的函式，是用來輸入一個組件來回傳一個要輸出的元素

```js
render: function(createElement){
    return createElement(App);
  }
```

這邊透過 Vue-cli 建立出來的

```js
render: h => h(App)
```

則是用箭頭函式又因為是只有一行又用 return ，所以把 return 省略，參數把小括號去掉，參數習慣用h取代。

### 單一檔案組件

把 javascript、html 模板、css 樣式都寫在同一檔案裡面。

如下面 App.vue 檔案中內容，名稱 App 就是在上面 Vue-cli 建立後 render: h => h(App) 帶入的檔案。

這邊 template 不是真的 html ，所以組件命名不一定要用 kebab case 方式命名。

```html
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>{{ msg }}</h1>
    <h2>Hello - vue-cli created</h2>
    <ul>
      <li><a href="https://vuejs.org" target="_blank">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank">Twitter</a></li>
    </ul>
    <h2>Ecosystem</h2>
    <ul>
      <li><a href="http://router.vuejs.org/" target="_blank">vue-router</a></li>
      <li><a href="http://vuex.vuejs.org/" target="_blank">vuex</a></li>
      <li><a href="http://vue-loader.vuejs.org/" target="_blank">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank">awesome-vue</a></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App - test'
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
```

![](https://i.imgur.com/PdXOqKQ.png)

### 解決 CSS 困境的 scoped style

- CSS 的選擇器是全域有效（global scope），沒辦法只在某些區域裡面運行
- 撞名- collision
- 加上前綴- specificity ，用槽狀方式一層一層選擇來增加權重

這些可以透過 vue 的單一檔案組件來解決。

外層 Outer.vue ， import Inner.vue 後有兩個 h1 ，要分別對這兩個 h1 做 style 設定的時候，在個別 vue 檔案組件中的 style 加上 scoped 後，在其中的 CSS 效果只會作用在剛檔案之中。
意思就是在不同組件中定義自己的 CSS 。

![](https://i.imgur.com/h49NOIW.png)

Outer.vue

```html
<template>
  <div>
    <h1>Outer</h1>
    <Inner/>
  </div>
</template>

<script>
import Inner from "./Inner.vue";
export default {
  components: {
    Inner
  }
};
</script>

<style scoped>
h1 {
  color: red;
}
</style>
```

Inner.vue

```html
<template>
  <div class="area">
    <h1>Inner</h1>
  </div>
</template>

<style scoped>
h1 {
  color: green;
}
</style>
```

但如果是在外層 Outer.vue 針對 div 來做 CSS 效果，內層雖然是個別組件都用上`style scoped`，因為 import Inner.vue 中的 div 也包含在外層 Outer.vue 的 div 中，所以也會一起影響到。
![](https://i.imgur.com/7OZwksY.png)

Outer.vue

```html
<template>
  <div class="area">
    <h1>Outer</h1>
    <Inner/>
  </div>
</template>

<script>
import Inner from "./Inner.vue";
export default {
  components: {
    Inner
  }
};
</script>

<style scope>
h1 {
  color: red;
}
.area {
  border: 2px blue solid;
}
</style>
```

Inner.vue

```html
<template>
  <div class="area">
    <h1>Inner</h1>
    <p></p>
    <input>
  </div>
</template>

<style scoped>
h1 {
  color: green;
}
</style>
```