---
title: "[Vue] - （四）列表渲染-實作"
date: 2019-04-29 18:00:00
tags:
  - Vue
categories:
  - project
---
## Demo

Lists Render - [Demo](https://orow.github.io/MyProjects/myVue/v-for-json-sideproject/index.html)

## 實作-json 資料渲染課程列表

從 data.json(用陣列包住)中用 ajax 方式獲取資料來渲染成列表。
![](https://i.imgur.com/NnYQ3ta.png)

data.json 格式範例

```json
{
    "img": "https://images.cakeresume.com/orow/7bd091b2-abc8-4dd6-85a8-850ba0d7802c.png",
    "url": "https://orow.github.io/MyProjects/ProjectsInJS&jQuery/YoutubeSearchEngine/index.html",
    "title": "Youtube Search Engine",
    "teacher": "AJAX-jQuery",
    "note": "https://orow.github.io/2019/03/16/jQuery-youtube-search/"
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
    <a :href="lesson.note">
      <span>筆記</span>
    </a>
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
