---
title: "[Vue] - （四）列表渲染"
date: 2019-04-29 15:00:00
tags:
  - Vue
categories:
  - Vue
---

## 列表渲染 Lists render

### v-for 把陣列轉為一組元素

舉例說明：`v-for="item in todo"`，前面參數item代表各個元素，後面todo代表整個陣列是陣列。
如果是從 1 開始到實際已知的數值可以不用宣告陣列。
也可以用 v-for 取 index 值：`v-for="(month, index) in months"`。

![](https://i.imgur.com/y8IPFLW.png)

```html
<div id="app">
  <ol>
    <!-- 前面參數代表各個元素，後面代表整個陣列 -->
    <li v-for="item in todo">{{item}}</li>
  </ol>
  <select>
    <!-- <option v-for="month in months">{{month}}</option> -->
    <!-- 如果知道陣列是從1到多少的實際數值，可以不用宣告陣列 -->
    <!-- <option v-for="month in 12">{{month}}</option> -->

    <!-- 取的每個元素的index -->
    <option v-for="(month, index) in months" :value="index+1">{{month}}</option>
  </select>
</div>
<script>
  new Vue({
    el: "#app",
    data: {
      todo: ["學會JavaScript", "學會Vue", "投履歷", "找到工作"],
      // 如果知道陣列是從1到多少的實際數值，可以不用宣告陣列
      // months:[1,2,3,4,5,6,7,8,9,10,11,12],

      // 陣列中不是數字，但option的name還是要帶數值，v-for中可以帶index
      months: ["Jan", "Feb", "Mar", "Apr", "May", "Jun"]
    }
  });
</script>
```

### v-for 把物件轉為一組元素

基本上用法與陣列轉換相同，但有些情況 v-for 在物件中不保證會按照宣告的順序呈現，ex.用數字宣告。
如果要按照順序，就使用陣列宣告。

![](https://i.imgur.com/JuApVLN.png)

```html
<div id="app">
  <!-- <div v-for="value in friend">{{value}}</div> -->

  <!-- 取key，v-for在物件中無法保證render後的順序，要照順序用array -->
  <!-- <div v-for="(value, key) in friend">{{key}}:{{value}}</div> -->

  <!-- render要照順序用array -->
  <div v-for="item in friend">{{item.prop}} - {{item.value}}</div>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      // friend: {
      //     name: 'Joe',
      //     id: 'Superman',
      //     age: '27',
      //     password: '12345',
      // },

      friend: [
        {
          prop: "name",
          value: "Joe"
        },
        {
          prop: "id",
          value: "Superman"
        },
        {
          prop: 32,
          value: "訂婚"
        },
        {
          prop: 28,
          value: "創辦公司"
        }
      ]
    }
  });
</script>
```

### 修改陣列或物件的注意事項

透過修改陣列與物件，結果會直接反應在 html 元素上，但也有一些狀況是不會直接反應在元素上。

```html
<div id="app">
  <ul>
    <!-- <li v-for="num in nums">{{num}}</li> -->
    <li v-for="(num, key) in nums">{{key}}: {{num}}</li>
  </ul>
  <button @click="action">Yo</button>
</div>
```

- 陣列修改：

    **能夠直接反應：**

    陣列的`push`, `pop`, `unshift`, `shift`, `splice`, `sort`, `reverse`，新增移除，由小到大排序，反轉等等都可以直接反應在元素上。`filter`會過濾後產生新陣列，也能夠直接反應。

    **不能夠直接反應：**

    用指定 index 的方式來操作，陣列會被改變，但 vue 沒辦法偵聽到這個變更，要即時反應在元素上可以用`splice`來操作。

    指定陣列的長度來修改也沒辦法直接反應在元素上，但可以透過`splice`或`slice`來操作即可。
    slice是區間取值 `slice(index1, index2) from index1 to index2`，算頭不算尾

```js
new Vue({
  el: "#app",
  data: {
    nums: [1, 2, 3, 4, 5, 6, 7, 8]

    // sort
    //nums:[1,3,5,4,2],
  },
  methods: {
    // 修改陣列
    action() {
      // 內容直接會反應在html元素上

      // this.nums.push(this.nums.length+1);
      // this.nums.pop();
      // this.nums.unshift(0);
      // this.nums.shift();

      // this.nums.splice(2,1);
      // 在該index2的位置刪除一個參數後，新增後面全部的數值
      // this.nums.splice(2,1,9,8,7);

      // 由小到大排序
      // this.nums.sort();

      // 反轉目前順序
      // this.nums.reverse();

      // filter 只留下偶數值，會生成新陣列
      // this.nums = this.nums.filter(elm => (elm%2 === 0));

      // 修改陣列內容「不會反應」在html元素上

      // 指定index的方式來操作，nums會被改變但是vue沒辦法偵聽到這變更
      // this.nums[0]= 10;
      // 用splice來指定index取代即可反應
      // this.nums.splice(0,1,10);

      // 指定陣列的長度修改也無法直接反應在元素上
      // this.nums.length =3 ;
      // 用splice或是slice即可反應在元素上
      // this.nums.splice(3);
      // 區間取值 slice(index1, index2) from index1 to index2，算頭不算尾
      this.nums = this.nums.slice(1, 3);
    }
  }
});
```

- 物件修改

    **不能夠直接反應：**
    指定的不存在物件的 key 的值也不會反應，指定已存在的物件是可以的。

    如果要設定修改一開始不存在的值，可以使用`$set`或`Vue.set`來修改，`$`字號為 vue 實例。
    其中有三個參數：該物件、key、value。

```js
new Vue({
  el: "#app",
  data: {
    nums: {
      x: 10,
      y: 20
    }
  },
  methods: {
    // 物件修改
    action() {
      // 指定的不存在物件的key的值也不會反應，指定已存在的物件是可以的
      // this.nums.z =30;
      this.nums.x = 111;

      // Vue中可以設定一開始不存在的值，$字號為vue實例
      // $set or Vue.set中有三個參數：該物件、key、value
      this.$set(this.nums, "z", 30);
      // Vue.set
      Vue.set(this.nums, "z", 30);
    }
  }
});
```

### 列表的過濾與排序

- reverse 反轉
- filter 過濾偶數
- sort 排序

要注意到 sort 會直接修改原本的陣列，可以先用 slice 來取陣列後 sorting。
![](https://i.imgur.com/DLa0e82.png)

```html
<div id="app">
  <ul>
    <li v-for="num in nums">{{num}}</li>
  </ul>
  <!-- Reverse -->
  <button @click="reverse">Reverse</button>
  <!-- All -->
  <button @click="all">All</button>
  <!-- Even -->
  <button @click="even">Even</button>
  <!-- Sort -->
  <button @click="sort">Sort</button>
</div>

<script>
  // const orgNums = [1,2,3,4,5,6,7,8,9];
  const orgNums = [5, 8, 1, 4, 3, 2, 9, 7, 6];
  new Vue({
    el: "#app",
    data: {
      // nums:[1,2,3,4,5,6,7,8,9],
      nums: orgNums
    },
    methods: {
      reverse() {
        this.nums.reverse();
      },
      even() {
        // this.nums = this.nums.filter(num => (num%2===0));
        this.nums = orgNums.filter(num => num % 2 === 0);
      },
      all() {
        this.nums = orgNums;
      },
      // sort會修改原本的陣列，用slice則不會影響原本陣列
      sort() {
        // this.nums = orgNums.sort();
        this.nums = orgNums.slice().sort();
      }
    }
  });
</script>
```

### v-for 渲染 template

一個元素不只要 render 出自己本身以外，可以用 template 模板（不是實際存在的元素）來渲染。

ex. 每個 h1 後要接上 hr 的標籤

```html
<div id="app">
  <!-- 一個元素不指要render出自己本身以外，可以用template模板 -->
  <!-- h1跟著hr -->
  <template v-for="header in headers">
    <h1>{{header}}</h1>
    <hr />
  </template>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      headers: ["About", "Products", "Contact"]
    }
  });
</script>
```

## 實作-json 資料渲染課程列表

從 data.json(用陣列包住)中用 ajax 方式獲取資料來渲染成列表。
![](https://i.imgur.com/Jsx19Uj.png)

data.json 格式範例

```json
{
    "img": "https://cdn.hiskio.com/courses/145/cover/1510133824V8glV.png",
    "url": "https://hiskio.com/course/145",
    "title": "精通 VueJS 之 前端開發完全指南 預購中",
    "teacher": "諾浩設計講堂"
},
```

用 v-for 迴圈帶出背景圖片、標題搭配 a 連結、課程老師。
要用背景圖來渲染，所以用上 v-bind 動態綁定屬性，a 連結的 href 也是動態綁定。

ajax 在 mounted 階段就去初始化，用原生的`fetch`來串接，之後再用`then`搭配箭頭函式，把值 return 到原本 vue 實例中 data 的空陣列裡面。

```html
<div id="app">
  <div v-for="lesson in lessons" class="lesson">
    <!-- <img :src="lesson.img" height="100"> -->
    <div class="image" :style="imgStyle(lesson.img)"></div>
    <a :href="lesson.url">
      <h1>{{lesson.title}}</h1>
    </a>
    <span class="teacher">{{lesson.teacher}}</span>
  </div>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      lessons: []
    },
    // 掛載的時候進行ajax獲取json資料
    mounted() {
      // fetch搭配then，then中使用箭頭函式
      fetch("./data.json")
        .then(res => res.json())
        .then(lessons => (this.lessons = lessons));
    },
    methods: {
      imgStyle(img) {
        return {
          // 'background-image':`url(${img})`,
          // Camel case
          backgroundImage: `url(${img})`
        };
      }
    }
  });
</script>
```

剩下的就是 css 的渲染設定

```css
html,
body {
  font-family: Arial, Microsoft JhengHei;
}
h1 {
  font-size: 18px;
}
a {
  color: #369;
  text-decoration: none;
}
.teacher {
  font-size: 12px;
}
.lesson {
  display: inline-block;
  width: 240px;
  border-radius: 8px;
  margin: 12px;
  padding: 8px;
  position: relative;
  height: 100px;
  padding-top: 150px;
  /* 圖片破壞圓角 */
  overflow: hidden;
  box-shadow: 4px 4px 20px rgba(0, 0, 0, 0.3);
}
.image {
  width: 100%;
  height: 150px;
  background-size: cover;
  background-position: center center;
  top: 0;
  left: 0;
  position: absolute;
}
```

## JSON 資料補充

json 中，物件的 key 需要用雙引號，物件中最後一個 key 或是 value 後不能有逗號，不能有函式（但可以宣告成字串，之後用 js 取出來用）。

`JSON.stringify()`括號中可以含三個參數，第一個是內容，第二個是 replacer，第三個是數字。

第二個 replacer，內容輸出後只會有第二個參數傳入陣列的內容會留下。

第三個數字，為了可讀性，斷行，縮排的次數。

```js
// const data = {
//     name: 'point',
//     x: 2,
//     y: 4,
// };
const dataJSON = {
  "name": "point",
  "x": 2,
  "y": 4
};

// 錯誤處理，try catch，try如果無法就會走catch
let data;
try {
  data = JSON.parse(dataJSON);
} catch (err) {
  data = {};
}
// const dataJSON = JSON.stringify(data);
// const data2 = JSON.parse(dataJSON);

// 輸出後只會有第二個參數傳入陣列的內容會留下
// 第二個參數replacer，第三個參數是為了可讀性，斷行，縮排的次數
const dataJSON = JSON.stringify(data, ["x", "y"], 2);

// 轉全部
const dataJSON = JSON.stringify(data, null, 2);
```
