---
title: "[Vue] - （三）HTML元素屬性綁定 & 條件判斷"
date: 2019-04-26 15:00:00
tags:
  - Vue
categories:
  - Vue
---

本篇主要是透過線上課程：`HiSKIO`、`w3school`及網路上搜尋資源所學習的。

## HTML 元素的屬性綁定

### 綁定資料為一個元素的 class

綁定 class 後可以回傳字串、物件、陣列。

input+button 的例子：
空白沒有輸入就是 submit disabled 的 class 效果（灰色），有輸入就是 submit 效果（綠色）。
![](https://i.imgur.com/4nYkftA.png)

用 computed 計算：

- if else 判斷後，return 字串或是者是物件
- 直接回傳物件

這邊在 data 中直接回傳綁定的 class 陣列，會直接綁定沒有判斷。

```html
<style>
  .submit {
    background: #28a745;
    color: white;
    border-radius: 8px;
    padding: 4px 8px;
    border: none;
  }
  .submit.disabled {
    background: #aaa;
  }
</style>

<div id="app">
  <input type="text" v-model="text" />
  <button v-bind:class="btnClass">Submit</button>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      text: ""
      //class綁定也可以直接用陣列，不用computed計算
      // btnClass: [
      //     'submit',
      //     'disabled'
      // ],
    },
    computed: {
      // if判斷，return回傳字串或物件
      // btnClass(){
      //     if(this.text.length === 0){
      //         // return 'submit disabled';
      //         // 可以return字串也可以是物件
      //         return {
      //             submit: true,
      //             disabled: true,
      //         }
      //     }else{
      //         // return 'submit';
      //         return {
      //             submit:true,
      //             disabled:false,
      //         }
      //     }
      // },

      // 簡潔做法，直接return物件
      btnClass() {
        return {
          submit: true,
          disabled: this.text.length === 0
        };
      }
    }
  });
</script>
```

### 綁定資料為一個元素的 style

- h1 標題 style 透過 Vue 綁定

在 data 中可以是字串、物件、陣列，陣列中要放的是物件，物件可以在外面宣告。
在物件裡面要用駝峰式命名（camel case），用`-`號程式會以為是相減。

- shrink 點擊縮小字體大小

![](https://i.imgur.com/0TZE93k.png)

shrink-1
data 中用物件，fontSize:'40px'，methods 中用 parseInt 轉為數字，再用字串模板`-1`來計算。

shrink-2
data 中直接給 size 數字，computed 中 return fontSize 搭配字串模板的 this.size 大小。
methods 中 shrink 就用`this.size--`來逐一的減一。

```html
<div id="app">
  <h1 v-bind:style="h1Style">Hello World!</h1>

  <!-- shrink縮小 -->
  <button v-on:click="shrink">OK</button>
</div>

<script>
  const commonHeadStyle = {
    fontSize: "20px"
    // 不能用font-size，-在物件中會被視為減號，所以要用駝峰式(camel case)命名
  };

  const yellowWord = {
    color: "yellow"
  };

  new Vue({
    el: "#app",
    data: {
      // 字串
      // h1Style: 'color:red',

      // 物件
      // h1Style:{
      //     color: 'green',
      // },

      // 陣列，裡面要放的是物件，物件可以在外面宣告
      // h1Style: [
      //     commonHeadStyle,
      //     yellowWord,
      // ],

      // 做一個點擊後會一直縮小字體大小
      // shrink-1
      // h1Style: {
      //     fontSize: '40px',
      // },

      // shrink-2
      size: 40
    },
    // shrink-2
    computed: {
      h1Style() {
        return {
          fontSize: `${this.size}px`
        };
      }
    },

    methods: {
      // shrink-1
      // shrink() {
      //     // replace 第二個參數轉為空白可以省略
      //     const size = parseInt(this.h1Style.fontSize.replace('px', ""), 10);
      //     this.h1Style.fontSize = `${size-1}px`;
      // },

      // shrink-2
      shrink() {
        this.size--;
      }
    }
  });
</script>
```

## 條件判斷

### v-if_else

點擊 checkbox 後顯示文字標題。
![](https://i.imgur.com/QPiSxKz.png)

在 html 中用 template 包住，template 是模板，不是真的元素。
v-else 只能接在 v-if 後面，而且要同一層，中間連`</br>`都不能插入

```html
<div id="app">
  <input type="checkbox" v-model="checked" />
  <!-- <h1 v-if="checked">跳樓大拍賣</h1> -->
  <!-- v-else只能接在v-if正下方且同一層，連插入</br>都不行 -->
  <!-- <h1 v-else>老闆不在家</h1> -->

  <!-- template不是真的元素，是一個模板用來包住其他元素 -->
  <template v-if="checked">
    <h1>跳樓大拍賣</h1>
    <h2>跳樓大拍賣</h2>
    <h3>跳樓大拍賣</h3>
  </template>
  <h1 v-else>老闆不在家</h1>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      checked: false
    }
  });
</script>
```

### v-else-if

除了二元以外各種選擇。
![](https://i.imgur.com/q1qokot.png)

```html
<div id="app">
  <input type="checkbox" v-model="checked1" />
  <input type="checkbox" v-model="checked2" />
  <h1 v-if="checked1">跳樓大拍賣</h1>
  <h1 v-else-if="checked2">夥計隨便賣</h1>
  <h1 v-else>老闆不在家</h1>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      checked1: false,
      checked2: false
    }
  });
</script>
```

### v-show

v-show 元素都會存在，差異在顯示與不顯示`display:none`。
v-if, v-else 等等，如果是 false 的話直接不 render 在 html 上。

v-show 不能搭配 v-else 跟 template，但 template 可搭配 v-if。

```html
<div id="app">
  <input type="checkbox" v-model="checked" />
  <!-- v-show只是顯示與不顯示，元素都存在 -->
  <!-- v-show不能搭配v-else，不能搭配template，但template可搭配v-if -->
  <h1 v-show="checked">跳樓大拍賣</h1>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      checked: false
    }
  });
</script>
```
