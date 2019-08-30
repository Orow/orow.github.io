---
title: "[Vue] - （九）自訂事件 Custom Events"
date: 2019-08-29 22:00:00
tags:
  - Vue
categories:
  - Vue
---

本篇主要是透過線上課程：`HiSKIO`、及網路上搜尋資源所學習的。

## 自訂事件 Custom events

### v-on 綁定自訂事件

在組件中發出自訂事件。

例子1
在組件中寫了一個 my-button ，渲染出來後是一個 button ，偵聽了 click 事件，執行 click 後就會用`this.$emit`發出`my-click`事件，就會被上層的 v-on 的 my-click 捕捉到，就會執行 clicked 函式。

$emit的第二個之後的參數是資料
![](https://i.imgur.com/9dHuCdB.png)

```html
<div id="app">
    <my-button v-on:my-click="clicked"></my-button>
</div>

<script>
    Vue.component('my-button',{
        template:`<button @click="myClick">my-button</button>`,
        methods:{
            myClick(){
                this.$emit('my-click', 100,200,300); //第一個參數是事件，第二個之後的參數是資料
            },
        },
    }),
    new Vue({
        el:'#app',
        methods:{
            clicked(x,y,z){
                alert('clicked' + `${x},${y},${z}`);
            },
        },
    })
</script>
```

例子2

內容有一個 +1 的 button 的 counter ，另一個 button 則是點擊後會執行 emit 函式。
在上層綁定下層傳來的`count-emit`事件，如果接收到事件後則執行 countEmit 函式。
![](https://i.imgur.com/0BlfsL6.png)

emit 函式執行時，會用 $emit 傳送事件到上層，綁定上層事件後執行 count 給值。
![](https://i.imgur.com/fDcH22E.png)

```html
<div id="app">
    <my-counter @count-emit="countEmit"></my-counter>
    <h1>{{count}}</h1>
</div>

<script>
    Vue.component('my-counter', {
            template: `
        <div>
            <h1>{{count}}</h1>
            <button @click="count+=1">+1</button>
            <button @click="emit">emit</button>
        </div>
        `,
            data() {
                return {
                    count: 0,
                }
            },
            methods: {
                emit() {
                    this.$emit('count-emit', this.count);
                },
            },
        }),
        new Vue({
            el: '#app',
            data: {
                count: 0,
            },
            methods: {
                countEmit(count) {
                    this.count = count;
                },
            },
        })
</script>
```

### 組件綁定原生事件

要偵聽原生事件的時候，一般的偵聽是不會有效果的，需要在事件後面加上`native`這個修飾符。
![](https://i.imgur.com/rTuKCf1.png)

ex. 

```html
<div id="app">
     <my-button @click="btnClicked"></my-button>
     <my-button @click.native="btnClicked"></my-button>
</div>

<script>
    Vue.component('my-button',{
        template:`
        <button>My Button</button>
        `
    }),
    new Vue({
        el:'#app',
        methods:{
            btnClicked(){
                alert('clicked');
            },
        },
    })
</script>
```

### .sync修飾符

例子說明

第一個功能
![](https://i.imgur.com/nXPYDA3.png)

下方計數數值為上層的 outerCount ，預設值為 10 。
上方計數數值為下層的 innerCount ，預設值想要綁定跟 outerCount 一樣的預設值 10 ，讓 +1 從 10 開始計算。
這邊需要用 props 上層傳到下層來綁定，需要注意命名， html 中只能用 kebab case 。

```html
<div id="app">
    <my-counter :outer-count="outerCount" @update="setOuterCount"></my-counter>
    <h1>{{outerCount}}</h1>
</div>

<script>
    Vue.component('my-counter',{
        props:['outerCount'], // html中要轉成kebab case
        data(){
            return{
                // innerCount:0,
                innerCount: this.outerCount,
            };
        },
        template: `
        <div>
            <h1>{{innerCount}}</h1>
            <button @click="innerCount+=1">+1</button>
            <button @click="sync">sync</button>
        </div>
        `,
        methods:{
            sync(){
                this.$emit('update', this.innerCount)
            },
        },
    })
    new Vue({
        el:'#app',
        data:{
            outerCount: 10,
        },
        methods:{
            setOuterCount(count){
                this.outerCount = count;
            },
        },
    })
</script>
```

第二個功能
 ![](https://i.imgur.com/VriItPf.png)

把下層的 innerCount 數值設定到上層的 outerCount 。
一般的作法則是用 this.$emit 把事件傳到上層，並偵聽更新的函式來將 outerCount 的數值更新即可。

在這邊則會用`.sync`，不需要偵聽update函式的方式來簡單的實現。

把上層的`@update="setOuterCount"`、`setOuterCount`的 method 拿掉， this.$emit 發送的事件改為`:update:outerCount`，原本綁定的 outer-count 加上 .sync 修飾符。

```html
<div id="app">
    <my-counter :outer-count.sync="outerCount"></my-counter>
    <!-- <my-counter :outer-count="outerCount" @update="setOuterCount"></my-counter> -->

    <h1>{{outerCount}}</h1>
</div>

<script>
    Vue.component('my-counter',{
        props:['outerCount'], // html中要轉成kebab case
        data(){
            return{
                // innerCount:0,
                innerCount: this.outerCount,
            };
        },
        template: `
        <div>
            <h1>{{innerCount}}</h1>
            <button @click="innerCount+=1">+1</button>
            <button @click="sync">sync</button>
        </div>
        `,
        methods:{
            sync(){
                this.$emit('update:outerCount', this.innerCount)
            },
        },
    })
    new Vue({
        el:'#app',
        data:{
            outerCount: 10,
        },
        // methods:{
        //     setOuterCount(count){
        //         this.outerCount = count;
        //     },
        // },
    })
</script>
```

`.sync`做的事情就是綁定 update 事件，把值回傳

```html
<div id="app">
    <!-- <my-counter :outer-count="outerCount" @update="setOuterCount"></my-counter> -->
    <my-counter :outer-count.sync="outerCount"></my-counter>
    <h1>{{outerCount}}</h1>
</div>

<script>
    Vue.component('my-counter',{
        props:['outerCount'], // html中要轉成kebab case
        data(){
            return{
                innerCount: this.outerCount,
            };
        },
        template: `
        <div>
            <h1>{{innerCount}}</h1>
            <button @click="innerCount+=1">+1</button>
            <button @click="sync">sync</button>
        </div>
        `,
        methods:{
            sync(){
                this.$emit('update:outerCount', this.innerCount)
            },
        },
    })
    new Vue({
        el:'#app',
        data:{
            outerCount: 10,
        },
    })
</script>
```

### 自訂組件的v-model

v-model 可以做到與 .sync 修飾符相似的效果。
v-model 就是用 v-bind 綁定 value 加上 v-on 綁定 input。

所以在組件中要針對 value 的屬性設定跟 $emit 一個 input 事件。
但 v-model 也可以在組件中指定一個 model 的屬性，裡面可以有兩個屬性 prop 跟 event ，兩者都是字串。
指定後，綁定的屬性(預設是 value )與事件名稱(預設是 input )就需要與指定後的名稱相同。

```html
<div id="app">
    <my-counter v-model="outerCount"></my-counter>
    <my-counter :count="outerCount" @update:count="val => outerCount = val"></my-counter>
    <!-- 等同v-model -->
    <!-- <my-counter :value="outerCount" @input="val => outerCount = val"></my-counter> -->

    <h1>{{outerCount}}</h1>
</div>

<script>
    Vue.component('my-counter', {
        // v-model綁定value與input以外還可以在組件中用model來指定prop跟event名稱
        model: {
            prop: 'count',
            event: 'update:count',
        },
        // props:['value'],
        props: ['count'],
        data() {
            return {
                // innerCount: this.value,
                innerCount: this.count,
            };
        },
        template: `
                <div>
                    <h1>{{innerCount}}</h1>
                    <button @click="innerCount+=1">+1</button>
                    <button @click="sync">sync</button>
                </div>
                `,
        methods: {
            sync() {
                // this.$emit('input', this.innerCount)
                this.$emit('update:count', this.innerCount)
            },
        },
    })
    new Vue({
        el: '#app',
        data: {
            outerCount: 10,
        },
    })
</script>
```

### 跨組件的溝通

兩個組件不是上下層，不是用 $emit 、 props 傳遞等等。如果兩個組件是同一層，沒有上下關係的話，則要用別的方式。

方式1
- 把 data 的 count 初始值設定移動到上層
- 偵聽 button 原生事件
- 綁定 count 屬性帶回 count 值

```html
 <div id="app">
    <my-count :count="count"></my-count>
    <my-button @click.native="count+=1"></my-button>

</div>

<script>
    Vue.component('my-count', {
            props:['count'],
            template: `
        <h1>{{count}}</h1>
        `
        }),
        Vue.component('my-button', {
            template: `
        <button>+1</button>
        `
        }),
        new Vue({
            el: '#app',
            data:{
                count:0,
            }
        })
</script>
```

方式2- vuex（後面單獨篇章）

方式1是比較簡單的方式，要注意的是，如果是要把 data 中的設定值保留在原本 component 裡面，或是很多個組件時就需要用 vuex 的方式來實現。
vuex會有統一的資料，會對每一個組件去做溝通，雙向綁定，是集中管理的概念。

方式3- event bus

先在最外層宣告共用的 event bus ，之後可以用這個 bus 來 $emit 發出事件， $on 偵聽事件等等。
$on 用來偵聽， $on 與 $emit 都是 vue 實例的一個方法。

偵聽 $on 需要在生命週期 mounted 的時期去偵聽，需要用到箭頭函式，因為箭頭函式的 this 是繼承而來，指的就是這個組件，如果不是用箭頭函式， this 則會指到 bus 這個 Vue 。

```html
<div id="app">
    <my-count></my-count>
    <my-button></my-button>
</div>

<script>
    const bus = new Vue();
    Vue.component('my-count', {
            data() {
                return {
                    count: 0,
                };
            },
            mounted() {
                // 偵聽外層的'add'事件
                bus.$on('add',() => {
                    // 用箭頭函式的this是繼承而來，指的就是這個組件，如果不是用箭頭函式，this則會指到bus這個Vue
                    this.count += 1;
                });
            },
            template: `<h1>{{count}}</h1>`,
        }),
        Vue.component('my-button', {
            // event bus
            template: `<button @click="click">+1</button>`,
            methods: {
                click() {
                    // 外層宣告的bus發出'add'事件
                    bus.$emit('add')
                },
            },
        }),
        new Vue({
            el: '#app',
        })
</script>
```