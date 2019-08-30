---
title: "[Vue] - （十）單一檔案組件 & Vue-cli - 實作"
date: 2019-08-30 16:00:00
tags:
  - Vue
categories:
  - project
---

本篇主要是透過線上課程：`HiSKIO`及網路上搜尋資源所學習的。

## 老套的 TodoList

![](https://i.imgur.com/STv0PYB.png)

vue的檔案組件中（App.vue），script 與 template 可以單獨存在，順序都可以，但 style 一定是要在最後，也可以不需要有 style 。

在使用單一檔案組件之後就會很少使用到`Vue.component`的定義方式。

這邊用到三個檔案，也就是三個組件的方式來寫。
第一層是 App.Vue ，在這裡會 import 其他兩個組件檔案。

**App.vue**

除了 import 其他兩個組件檔案以外，還有最外層的 html 模板、todos 陣列、 addTodo 、 removeTodo 的 methods 等等。
這邊要注意到 removeTodo 的 click 事件需要偵聽原生的事件，所以要加上 native 。

```html
<script>
import TodoInput from "./TodoInput";
import TodoItem from "./TodoItem";

export default {
  components: {
    TodoInput,
    TodoItem,
  },
  data() {
    return {
      todos: []
    };
  },
  methods: {
    addTodo(text) {
      this.todos.push(text);
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    }
  }
};
</script>

<template>
  <div>
    <TodoInput @submit="addTodo"/>
    <ul>
      <TodoItem 
        v-for="(todo, index) in todos"
        :data="todo"
        :key="index"
        @click.native="removeTodo(index)"/>
    </ul>
  </div>
</template>

<style>
</style>
```

**TodoInput.vue**

模板中是用 form 包含 input 跟 submit 的 button 。
data 中需要 text 用來雙向綁定輸入的值、 submit 的 method ，這邊 sumbit 需要把 sumbit 的事件 $emit 到上層去，讓上層去執行 addTodo 的 method 。

```html
<template>
  <form @submit.prevent="submit">
    <input v-model="text"/>
    <button type="sumbit">Submit</button>
  </form>
</template>

<script>
export default {
    data(){
        return {
            text: '',
        };
    },
    methods:{
        submit(){
            this.$emit('submit', this.text);
            this.text = "";
        },
    },
}
</script>
```

**TodoItem.vue**

綁定 props 的 data ，因為在模板中渲染出每一個項目的內容，所以要綁定上層傳來的 data 。

```html
<script>
export default {
    props:['data'],
}
</script>

<template>
    <li class="todo-item">
       {{data}} 
    </li>
</template>

<style>
    .todo-item{
        font-size: 1.1rem;
        font-family: Arial;
        background-color: #fed;
        margin: 6px 0;
        padding: 4px 8px;
        cursor: pointer; 
    }

    .todo-item:hover{
        background-color: #def;
    }
</style>
```