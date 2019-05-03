---
title: "[Vue] - （六）表單綁定"
date: 2019-05-01 15:00:00
tags:
  - Vue
categories:
  - Vue
---

## 表單綁定 Event handling

### text input &textarea

在 input 的 v-model 其實就是 v-bind:value 加上偵聽 input 事件 v-on:input。

textarea 中則是可以多行輸入，但 html 上顯示一般來說一樣是單行顯示，如果要在 html 上保留原本的換行字元。將綁定元素的標籤改為`pre`，preserve 即可，或者在 css 中用`white-space: pre;`
![](https://i.imgur.com/0jMJ5ts.png)

```html
<style>
  h1 {
    white-space: pre;
  }
</style>

<div id="app">
  <!-- v-model =  v-bind:value + v-on:input-->
  <input type="text" v-model="msg" />
  <input type="text" :value="msg" @input="setMessage" />
  <h1>{{msg}}</h1>
  <!-- 多行，pre標籤會保存換行字元 -->
  <textarea v-model="message"></textarea>
  <pre>{{message}}</pre>
  <h1>{{message}}</h1>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      msg: "",
      message: ""
    },
    methods: {
      setMessage(evt) {
        console.log(evt);
        this.msg = evt.target.value;
      }
    }
  });
</script>
```

### checkbox

checkbox 可以單選綁定，也可以多選綁定。

單選-確認選擇 agree 後，submit 才會成功。
![](https://i.imgur.com/eZe0uW0.png)

```html
<div id="app">
  <input type="checkbox" id="check" v-model="agree" />
  <label for="check">Agree</label>
  <h1>{{agree}}</h1>
  <button @click="submit">Submit</button>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      agree: false
    },
    methods: {
      submit() {
        if (this.agree !== true) {
          alert("Please agree!");
          return;
        }
      }
    }
  });
</script>
```

多選-v-model 綁定相同名稱，該名稱宣告為陣列，但順序是依照勾選的順序來呈現。
![](https://i.imgur.com/Y3oG01f.png)

```html
<div id="app">
  <input type="checkbox" id="check1" value="morning" v-model="times" />
  <label for="check1">Morning</label>
  <input type="checkbox" id="check2" value="afternoon" v-model="times" />
  <label for="check2">Afternoon</label>
  <input type="checkbox" id="check3" value="evening" v-model="times" />
  <label for="check3">Evening</label>
  <h1>{{times}}</h1>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      times: []
    }
  });
</script>
```

### radio

透過 v-model 綁定相同的名稱，來達成單選，技術上可以達成多選，但不建議，要多選建議用 checkbox。
![](https://i.imgur.com/0VtleIZ.png)

```html
<div id="app">
  <!-- 透過v-model綁定相同名稱來達成單選 -->
  <input type="radio" id="r1" v-model="gender" value="male" />
  <label for="r1">Male</label>
  <input type="radio" id="r2" v-model="gender" value="female" />
  <label for="r2">Female</label>
  <h1>{{gender}}</h1>
</div>
<script>
  new Vue({
    el: "#app",
    data: {
      gender: "male"
    }
  });
</script>
```

### select & option

下拉選單綁定

單選

```html
<div id="app">
  <select v-model="year">
    <!-- 一開始會選擇value=""為預設選擇 -->
    <option disabled value="">Select Year</option>
    <!-- 如果沒有value，被選到會直接顯示文字 -->
    <option>2015</option>
    <option value="16">2016</option>
    <option value="17">2017</option>
  </select>
  <h1>{{year}}</h1>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      year: ""
    }
  });
</script>
```

多選，用 multiple，綁定陣列。

```html
<div id="app">
  <select v-model="years" multiple>
    <option disabled value="">Select Year</option>
    <option>2015</option>
    <option value="16">2016</option>
    <option value="17">2017</option>
  </select>
  <h1>{{years}}</h1>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      years: []
    }
  });
</script>
```

不把每一個 option 打出來時，用 v-for

- data 中宣告陣列，宣告一開始 v-model 預設數值
- v-for 中的陣列用數字帶入，data 中不需樣宣告陣列
- data 中宣告的陣列每一個都為物件，value 與 text 分別取用。

```html
<div id="app">
  <!-- 3 -->
  <select v-model="selectedYear">
    <option v-for="year in years" :value="year">{{year}}</option>
  </select>
  <h1>{{selectedYear}}</h1>

  <!-- 4 -->
  <select v-model="selectedMonth">
    <option v-for="month in 12" :value="month">{{month}}</option>
  </select>
  <h1>{{selectedMonth}}</h1>

  <!-- 5 -->
  <select v-model="selectedGender">
    <option v-for="option in options" :value="option.value"
      >{{option.text}}</option
    >
  </select>
  <h1>{{selectedGender}}</h1>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      // 3
      years: [2015, 2016, 2017],
      selectedYear: 2015,

      // 4
      selectedMonth: 1,

      // 5
      options: [
        { value: 1, text: "Male" },
        { value: 2, text: "Female" },
        { value: 3, text: "Others" }
      ],
      selectedGender: 1
    }
  });
</script>
```

### 檔案上傳與圖片預覽

檔案上傳的圖片預覽，input type="file" 綁定 v-model 是沒有用的，需要綁定 chagne 事件偵聽。

如果 type 是 file，事件觸發傳入的 event 裡面的 target 會有 files，是以 file list 呈現。
之後再 new 一個`fileReader()`，針對這個 reader 偵聽 load 事件，並用 readAsDataURL 轉為 data:URL（base64 編碼），偵聽 load 事件後執行的函式,則是把事件參數的 evt.traget.result 指回到 vue data 中的 image。
![](https://i.imgur.com/mleWDbR.png)

```html
<div id="app">
  <input type="file" @change="fileSelected" />
  <br />
  <img v-if="image" :src="image" width="200" />
  <br />
  <button @click="upload">Upload</button>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      image: "",
      // 檔案上傳要在data中宣告file
      file: null
    },
    methods: {
      fileSelected(evt) {
        // 圖片預覽
        // const file = evt.target.files.item(0);
        // 檔案上傳
        this.file = evt.target.files.item(0);
        const reader = new FileReader();
        reader.addEventListener("load", this.imageloaded);
        // 將屬性result轉完data:URL（base64編碼）
        reader.readAsDataURL(this.file);
      },
      imageloaded(evt) {
        console.log(evt);
        this.image = evt.target.result;
      }
    }
  });
</script>
```

檔案上傳的方式不一定，基本上是後端而定。

偵聽點擊 upload 事件

```js
// 上傳方式視後端而定
    upload() {
        // bae64字串直接上傳
        axios.post('/upload', {
            image: this.imgage
        });

        // formdata
        const form = new FormData();
        form.append(this.file, this.file.name);
        axios.post('/upload', form);
    },
```

### 語法糖修飾符 modifier

- lazy: blur 離開焦點的時候才會觸發綁定
- number: 輸入的值當成一個數字，原本是字串
- trim: 輸入的值前後的空格裁掉

```html
<div id="app">
  <!-- lazy，blur時才會觸發雙向綁定 -->
  <input type="text" v-model.lazy="msg" />
  <h1>{{msg}}</h1>

  <!-- number，輸入後會當成數字，沒有加則會變成字串 -->
  <input type="text" v-model.number="num" />
  <h1>{{num+1}}</h1>

  <!-- trim 自動省略前後的空白 -->
  <input type="text" v-model.trim="text" />
  <h1>[{{text}}]</h1>
</div>

<script>
  new Vue({
    el: "#app",
    data: {
      msg: "",
      num: 0,
      text: ""
    }
  });
</script>
```

## 實作-後台-課程上架

後端 server-從課程上取得程式，複製到 local 端去跑 server。

課程上架列表用 form 表單，內容有

- Title
- URL
- Teacher
- Completed
- Image
- 圖片預覽
- submit 按鈕

![](https://i.imgur.com/Yv7Aqp1.png)

顯示內容則透過 get 資料指定回 data 中的陣列，再用 v-for 動態綁定顯示各個資料。
![](https://i.imgur.com/zeMEgtH.png)

methods 中的 fileSelected 跟 imgloaded 是圖片預覽所需。
submit 則是點擊 submit 後要執行的方法，會將雙向綁定的各個值與選擇的檔案指定到 vue 實例的 data 裡面，再送到後端。

檔案上傳限定圖片可以使用`accept="image/*"`。

在生命週期的 mounted 則會先從後端 get 現有資料，再把回傳的資料指定回 vue 實例中 data 的 lessons 陣列中。

```html
<div id="app">
  <form @submit.prevent="submit">
    <label>Title</label>
    <input type="text" v-model="title" />
    <br />
    <label>URL</label>
    <input type="text" v-model="url" />
    <br />
    <label>Teacher</label>
    <select v-model="teacher">
      <option v-for="teacher in teachers">{{teacher}}</option>
    </select>
    <br />
    <label>Completed</label>
    <input type="checkbox" v-model="completed" />
    <br />
    <label>Image</label>
    <input type="file" accept="image/*" @change="fileSelected" />
    <br />
    <img v-if="img" :src="img" width="200" />
    <button type="submit">Submit</button>
  </form>
  <hr />
  <div>
    <div v-for="lesson in lessons">
      <img :src="lesson.image" width="100" />
      <a :href="lesson.url">
        <span>{{lesson.title}}</span>
      </a>
      <span>{{lesson.teacher}}</span>
      <span>{{lesson.completed == 'true'? '完成' : '未完成'}}</span>
    </div>
  </div>
</div>

<script>
  const URI = "local端的server位址";
  new Vue({
    el: "#app",
    data: {
      title: "",
      url: "",
      teachers: ["Scars", "Jason", "Paul", "Roy"],
      teacher: "",
      completed: false,
      img: "",
      lessons: []
    },
    methods: {
      fileSelected(evt) {
        const file = evt.target.files.item(0);
        const reader = new FileReader();
        reader.addEventListener("load", this.imgloaded);
        reader.readAsDataURL(file);
      },
      imgloaded(evt) {
        this.img = evt.target.result;
      },
      submit() {
        $.post(URI, {
          title: this.title,
          url: this.url,
          teacher: this.teacher,
          completed: this.completed,
          image: this.img
          // 這邊不需要then去清除內容，在submit後會自己清除
          // }).then(() => {
          //     this.title = '';
          //     this.url = '';
          //     this.teacher = "";
          //     this.completed = false;
          //     this.img = "";
          //     this.loadLessons();
        });
      }
      // 目前不需要此function即可動態看到結果
      // loadLessons(){
      //     $.get(URI).then(lessons => this.lessons = lessons);
      // },
    },
    mounted() {
      // then()，promise用法
      $.get(URI).then(lessons => (this.lessons = lessons));
      // 沒有去清除內容後接著get資訊即可看到
      // this.loadLessons();
    }
  });
</script>
```
