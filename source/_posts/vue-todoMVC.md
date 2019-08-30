---
title: "[Vue] - todoMVC-待辦事項功能-實作"
date: 2019-05-06 18:00:00
tags:
  - Vue
categories:
  - project
---

## Demo

todoMVC-vue 的應用- [Demo](https://orow.github.io/todoMVC-vue/index.html)

## todoMVC 分析

學習大約 10 天的 vue 後，嘗試製作入門的 todoMVC 功能。這次實作著重在於功能的撰寫，把 todoMVC 官網上的版本 clone 下來，我是用 jQuery 的版本來修改，刪除 js 所有檔案，保留 html 靜態網頁的部分、css 版面等等，再引入 vue 的 cdn。

**功能分析**

先分析有哪些功能，怎麼觸發功能，再思考怎麼去執行。
主要有七個功能：

- 新增待辦項目
- 刪除項目功能
- 修改項目狀態
- 選取（取消選取）全部
- 清除完成項目
- 編輯已存在的項目
- 篩選狀態後顯示項目

#### 新增待辦項目

輸入後按下 enter 即可新增，綁定`keyup.enter`事件。

![](https://i.imgur.com/astvRxJ.png)

#### 刪除待辦項目

移動到已存在的項目後，右方會有 x 按鈕，點擊會刪除該項目。
預計用`splice`來刪除選取到的陣列中該 index 的項目。

![](https://i.imgur.com/GCPG68C.png)

#### 修改項目狀態

點擊該按鈕，狀態會在執行中跟已完成切換，下方的未完成的數量會根據狀態來計算。
數量的部分，自己分析是在 vue 實例中給一個長度 0，新增項目就+1，切換狀態到已完成或是刪除就-1。

![](https://i.imgur.com/Fh03D93.png)

#### 選取（取消選取）全部

點擊選取全部或是取消選取全部。

![](https://i.imgur.com/5KbPlgI.png)

#### 清除已完成項目

刪除是狀態已完成的項目，分析用 for 迴圈來執行全部待辦事項的陣列，判斷狀態是已完成就刪除。

![](https://i.imgur.com/nqfTzWe.png)

#### 編輯已存在的項目

click 後會進入編輯狀態，這部分一開始只有分析出，點擊後會帶入 class 來產生顯示編輯狀態，按 enter 會送出，esc 會離開不修改。

![](https://i.imgur.com/86dawq4.png)

#### 篩選狀態後顯示項目

task 有兩種狀態，active 執行中與 completed 完成，選擇 all 則是全部狀態都顯示，一開始預設是 all 顯示。
這部分認為是用 v-if, v-else 來顯示，具體做法還不確定，先執行其他功能。

![](https://i.imgur.com/TGFz2mL.png)

選擇 active，判斷狀態是進行中的 task
![](https://i.imgur.com/YAKj1rT.png)

completed，判斷是已選取的 task
![](https://i.imgur.com/0OsX4LV.png)

## 功能

### 新增待辦項目

保留原 html 內容，`v-model`綁定`inputValue`，`v-on:keyup.enter="add"` 綁定 keyup 事件執行新增項目的 function。

```html
<input
  class="new-todo"
  type="text"
  v-model="inputVaule"
  @keyup.enter="add"
  placeholder="Add to do item..."
  autofocus
/>

<script>
  data:{
      inputValue: '',
      items: [],
      todoLen: 0, // 未完成項目預設為0，
  },
  methods:{
    add() {
        let todoContent = {
            text: this.inputVaule,
            completed: false,
        };
        // 把新增項目的內容，跟預設false的狀態放在物件中push到陣列中
        this.items.push(todoContent);
        // 這個長度是用來計算未完成項目的數量
        this.todoLen++;
        this.inputVaule = '';
  }
</script>
```

### 刪除待辦項目

顯示的待辦項目中，button 綁定 click 事件來執行刪除項目的函式。
數量的部分，自己分析是在 vue 實例中給一個長度 0，新增項目就+1，切換狀態到已完成或是刪除就-1。

```html
<ul class="todo-list">
  <template v-for="(item, index) in items">
    <!-- 顯示待辦項目 -->
    <li :class="{completed: item.completed}">
      <div class="view">
        <input class="toggle" type="checkbox" />
        <label>{{item.text}}</label>
        <button class="destroy" @click="removeTodo(index)"></button>
      </div>
    </li>
  </template>
</ul>

<script>
  methods:{
    // 點擊事件觸發的函式要回傳index，才會刪除正確的項目
    removeTodo(index) {
    // 多一個判斷，如果選中的項目是未完成，要把數量減一
    if (this.items[index].completed == false) {
        this.todoLen--;
    }
    // 如果本來就是已完成，不需要減少數量
    this.items.splice(index, 1);
    },
  }
</script>
```

### 修改項目狀態

點擊該按鈕，狀態會在執行中跟已完成切換。用 v-model 綁定項目狀態，再綁定 change 事件來執行函式。
這邊要注意的是，如果項目原本是未完成的，點擊時會因為 v-model 綁定狀態，會把項目的狀態改回 true 也就是已完成。這時候 change 事件執行的函式，就需要判斷如果是改變後的狀態是 true，計算未完成的數量要減一。
反之原本是已完成的項目，點擊後，狀態改為未完成、數量加一。

```html
<ul class="todo-list">
  <template v-for="(item, index) in items">
    <li :class="{completed: item.completed}" v-else>
      <div class="view">
        <!-- v-model綁定該項目的狀態，綁定change事件執行函式 -->
        <input
          class="toggle"
          type="checkbox"
          v-model="item.completed"
          @change="changeTodo(index)"
        />
        <label>{{item.text}}</label>
        <button class="destroy" @click="removeTodo(index)"></button>
      </div>
    </li>
  </template>
</ul>

<script>
  methods:{
      changeTodo(index) {
      	// 因為v-model綁定completed的狀態，在changeTodo函式中，需要以改變後的狀態來判斷
        // 原本是未完成，false
      if (this.items[index].completed == true) {
          this.todoLen--;
      } else {
          // 原本是已完成，改為未完成，所以要加一
          this.todoLen++;
      }
    },
  }
</script>
```

### 選取（取消選取）全部

點擊選取全部或是取消選取全部。綁定點擊執行 selectAll 函式。
判斷如果現在未完成數量等於 0，用 for 迴圈把陣列 items 中每一個項目的狀態都改為 false，然後把未完成數量改為陣列的長度。
如果未完成數量不等於 0，也用 for 迴圈把每一個項目的狀態改為 true 以達成全選的效果，未完成數量則改為 0。

```html
<section class="main">
  <input
    id="toggle-all"
    class="toggle-all"
    type="checkbox"
    @click="selectAll"
  />
  <label for="toggle-all">Mark all as complete</label>
  <ul class="todo-list">
    <li>...</li>
    ....
  </ul>
</section>

<script>
  selectAll() {
      // 如果都是已完成項目，把全部狀態改為false，數量等於陣列長度
      if (this.todoLen == 0) {
          for (let i = 0; i < this.items.length; i++) {
              this.items[i].completed = false;
          }
          this.todoLen = this.items.length;
      } else {
          // 只要數量不等於0，會把所有項目狀態改為true，達到全選效果
          for (let i = 0; i < this.items.length; i++) {
              this.items[i].completed = true;
          }
          this.todoLen = 0;
      }
  },
</script>
```

### 清除已完成項目

刪除是狀態已完成的項目，用 for 迴圈來執行全部待辦事項的陣列，判斷狀態是已完成就刪除。
在實作的時候發現，執行時會有異常，狀態有些項目突然變成已完成等等。
原因是 splice 執行時會改變陣列長度，所以只有在不刪除元素狀況下才會去執行行 i++。

```html
<!-- 這邊v-if判斷如果項目陣列中有待辦項目就會顯示 -->
<footer class="footer" v-if="items != ''">
  <span class="todo-count"><strong>{{todoLen}}</strong> item left</span>
  <ul class="filters">
    <li>...</li>
    ...
  </ul>
  <button class="clear-completed" @click="clearCompleted">
    Clear completed
  </button>
</footer>

<script>
  methods:{
      clearCompleted() {
        for (let i = 0; i < this.items.length;) {
            if (this.items[i].completed == true) {
                // splice會改變陣列長度，所以只有在不刪除元素狀況下才會去執行行i++
                this.items.splice(i, 1);
            } else {
                i++;
            }
        }
    },
  }
</script>
```

### 編輯已存在的項目

在實作編輯功能的時候，卡關很久，網路上搜尋資源，很多方式來達成，但因為前面功能自己寫，跟網路上做法有些差異，後來網路上找到別的相對簡單方式來實現。
分析的時候，這個 css 配置是 li ＋ input 搭配`li class="editing"`就會轉換成編輯模式。

- [參考來源](https://www.zmrenwu.com/courses/vue2x-todo-tutorial/materials/44/)

用 click 來觸發編輯模式，double click在mobile上不支援。

#### v-if 判斷顯示編輯模式

另外還需要 v-if 來判斷顯示，在 vue instance 的 data 新增一個`editedItem: null`，這是用來暫存編輯項目的原本內容，還用來判斷顯示編輯模式會取用的值。

如果 editedItem 不是 null，就會顯示編輯模式，所以在 click 執行的函式中，會把原本項目的內容存到這裏來，這樣編輯模式也會顯示。原本的待辦項目顯示用 v-else 則不會渲染出來。

註：這邊 v-if 也需要判斷暫存資料（editedItem）中的 index 跟目前選取的項目 index 是否相同，這樣編輯模式才會一次顯示一個。

html：

```html
<ul class="todo-list">
  <template v-for="(item, index) in items">
    <!-- 編輯項目顯示 -->
    <li
      class="editing"
      v-if="editedItem !== null && editedItem.index === index"
    >
      <input
        class="edit"
        v-model="item.text"
        @keyup.enter="editFinished(item)"
        @keyup.esc="cancelEdit(item)"
        v-focus="true"
      />
    </li>
    <!-- 待辦項目顯示 -->
    <li :class="{completed: item.completed}" v-else>
      <div class="view">
        <input .... />
        <label @click="editItem(item, index)">{{item.text}}</label>
        <button ...></button>
      </div>
    </li>
  </template>
</ul>
```

#### 編輯的部分需要三個功能

- 在原本項目上 click 後執行
- 編輯完成後 keyup.enter 執行函式
- 取消編輯，keyup.esc 執行函式

**Click 後執行函式**

觸發事件後執行`editItem(item, index)`。
把 data 中要當作暫存的 editedItem 指定為目前項目的內容、狀態，index 也加起來是為了判斷編輯模式只會顯示在正確的項目。
這邊不用`this.editedItem = item`，是因為這樣會變成直接引用，只要 item 有修改，暫存區的內容也會跟著修改。

```js
data: {
          editedItem: null, // 單一項目編輯的暫存，編輯的v-if判斷
          },
  methods:{
          // editedItem是用來點擊後暫存編輯前的資料，不直接引入項目名稱是因為會連動，所以單純保存資料
      editItem(item, index) {
          // 多了一個index是要在html中用來判斷v-if顯示點擊的單一項目來編輯
          this.editedItem = {
              index: index,
              text: item.text,
              completed: item.completed
          };
      },
  }
```

**編輯完成按下 enter 後執行函式**

在編輯模式的 input 中用了 v-model 綁定`item.text`，輸入會直接綁定 item 的內容。
按下 enter 則是會把用來暫存的 editedItem 清空改為 null，也會讓編輯模式的 v-if 條件變為 false 而不渲染出來。

```js
  methods:{
      // 編輯時按下enter，會直接清空暫存資料，v-if會等false就不顯示，內容直接綁定v-model
      editFinished(item) {
          this.editedItem = null
      },
    }
```

**取消編輯，按下 esc 後執行函式**

Click 後進入編輯模式後，尚未編輯或者是編輯到一半，因為 v-model 綁定輸入值，所以按下 esc 後要從暫存的地方取回資料後再去清空改為 null。

```js
  methods:{
      // 編輯時在按下enter前要取消，按下esc會執行
      // 因為v-model綁定輸入值，所以要從暫存區取回資料
      cancelEdit(item) {
          item.text = this.editedItem.text;
          this.editedItem = null;
      },
      }
```

**補充：進入編輯模式後，自動 focus 在 input 上**

在 html 的 input 中加上`v-focus="true"`。
這樣在 click 顯示編輯模式的時候也會直接 focus 在 input 上。

```js
// auto focus，下方寫完後，在html中需要自動focus的元素加上v-focus="true"
directives: {
    focus: {
        inserted(el) {
            el.focus()
        }
    }
},
```

### 篩選狀態後顯示項目

篩選的這個功能也是卡關很久，也是參考跟編輯功能相同資源來學習。

- [參考來源](https://www.zmrenwu.com/courses/vue2x-todo-tutorial/materials/48/)

在 vue instance 的 data 中新增 filterTask，預設用`'all'`，意思就是顯示全部。再用 computed 來計算屬性（fitlerItems），在 html 透過綁定 click 事件來修改 filterTask 的內容，進而觸發 computed 的屬性計算。

computed 的 filterItems，在判斷 data 中 filterTask 的值後再用 filter 來 return 值。
如果是`'active'`，return 的則會是只有 item.completed = false 的 item。
如果是`'completed'`，return 的則會是只有 item.completed = true 的 item。
其餘的（也就是`'all'`），就會 return 在 vue instance 中的待辦項目陣列。

用 v-for 來顯示待辦項目中的陣列也改為 computed 中的 filterItems 來渲染。

另外每一個篩選的按鈕要綁定 class，用三元判斷式來判斷 filterTask 的內容，如果符合=true，就回傳`selected`。

```html
<section class="main">
  <ul class="todo-list">
    <!-- v-for 渲染的陣列改為computed中的filterItems -->
    <template v-for="(item, index) in filterItems">
      <li>...</li>
    </template>
  </ul>
</section>
<footer class="footer" v-if="items != ''">
  <span class="todo-count"><strong>{{todoLen}}</strong> item left</span>
  <ul class="filters">
    <li>
      <a
        href="#/all"
        :class="(filterTask == 'all')? 'selected' : ''"
        @click="filterTask ='all'"
        >All</a
      >
    </li>
    <li>
      <a
        href="#/active"
        :class="(filterTask == 'active')? 'selected' : ''"
        @click="filterTask ='active'"
        >Active</a
      >
    </li>
    <li>
      <a
        href="#/completed"
        :class="(filterTask == 'completed')? 'selected' : ''"
        @click="filterTask ='completed'"
        >Completed</a
      >
    </li>
  </ul>
  <button class="clear-completed" @click="clearCompleted">
    Clear completed
  </button>
</footer>

<script>
   data:{
       filterTask: 'all', // 用來篩選項目，預設為顯示全部
   },
   // 計算屬性，判定篩選項目
  computed: {
  	filterItems() {
  		if (this.filterTask === 'active') {
  			return this.items.filter(item => !item.completed)
  		} else if (this.filterTask === 'completed') {
  			return this.items.filter(item => item.completed)
  		} else {
  			// 剩下就是'all'
  			return this.items
  		}
  	},
  },
</script>
```

### 資料存進 localsotrage

做完上面這些功能，可以說是完成了，但只要網頁一重新整理，全部的待辦事項都會清空了，所以這時候就需要使用瀏覽器中的 localstorage 來存取。

使用 js 語法，把儲存與獲取的兩個 function 指定在一個物件裡面，方便取用。
要注意，存進 localstorage 時要轉為字串，取用時要用 parse 把字串解析回來。

```js
const storageLists = "toDoMVC-vue";
let todoStorage = {
  getToDos() {
    // 如果一開始localstorage中沒有資料，會取得[]空陣列
    let todos = JSON.parse(localStorage.getItem(storageLists) || "[]");
    return todos;
  },
  saveToDos(item) {
    localStorage.setItem(storageLists, JSON.stringify(item));
  }
};
```

在設定好 localstorage 存取的函式後，就可以來設定什麼時候存取了。
vue instance data 中的 itmes 待辦項目陣列，直接指定執行獲取 localstorage 的函式。
watch 偵聽 items 陣列，只要 items 中有資料變更，就會存到 localstorage 中。

```js
data: {
    items: todoStorage.getToDos(), // 在建立vue instance的時候直接取資料
},
// 用watch偵聽items陣列，只要items中有資料變更，就會存到localstorage中
watch: {
    items: {
        handler(items) {
            todoStorage.saveToDos(items);
        },
        deep: true // 偵聽陣列
    },
},
```

### mounted 執行未完成項目數量的初始化

 在左下角會顯示剩餘未完成項目的數量，在更新網頁時會歸零，因為在 vue data 中預設值是 0。
為了處理這個狀況，在 methods 中寫了一個初始化數量的函式，用 for 迴圈來判斷整個待辦項目陣列的長度後，再來判斷狀態為 false 的話，數量參數 todoLen 就要+1。

並在 vue 中的 mounted 中去執行這個函式，這樣才每次網頁重新整理建立 vue 的時候就會執行。

```js
methods:{
    initTodoLen() {
        let itemsLen = this.items.length;
        for (let i = 0; i < itemsLen; i++) {
            if (this.items[i].completed === false) {
                this.todoLen++;
            }
        }
    },
},
mounted() {
    this.initTodoLen();
},
```
