---
title: "[Vue] - （六）表單綁定-實作"
date: 2019-05-01 18:00:00
tags:
  - Vue
categories:
  - project
---

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

內容輸入完成，選好圖片，顯示預覽
![](https://i.imgur.com/IdmBsBm.png)


按下submit送出ajax post後，ajax get回來後渲染出來。
![](https://i.imgur.com/w19Z6eG.png)


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
      teachers: ["Orow", "Jason", "Paul", "Roy"],
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