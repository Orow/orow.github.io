---
title: "[Vue] - （八）屬性 Props"
date: 2019-08-29 12:00:00
tags:
  - Vue
categories:
  - Vue
---

本篇主要是透過線上課程：`HiSKIO`、及網路上搜尋資源所學習的。

## 屬性 Props

### 從上層傳來的prop

Vue 組件裡面會用到上層傳來的 props ，上層傳來的屬性也需要宣告。
命名的部份，在 html 裡面只有能用 kebab case 的方式，與組件命名相同。

在 Vue 實例中可以用 camel case或是 pascal-case 來命名。

```js
<div id="app">
    <my-component my-message="hello" test="test"></my-component>
</div>


<script>
    Vue.component('my-component', {
        props: ['myMessage', 'test'], // 上層傳來的屬性需要宣告
        template: '<h1>{{myMessage}}{{test}}</h1>'
    });
    new Vue({
        el: '#app',
    })
</script>
```

但如果是透過字串模板不是用 html 模板的話命名上就沒有只能用 kebab case 這個限制。

### 動態綁定Prop

用 v-bind 綁定屬性

```html
<div id="app">
    <post :text="text"></post>
</div>

<script>
    new Vue({
        el: '#app',
        data:{
            text: 'hello',
        },
        components: {
            post: {
                props: ['text'],
                template: '<div>{{text}}</div>'
            },
        },
    });
</script>
```

用 v-for 把 data 中整個陣列（字串）秀出來
![](https://i.imgur.com/xXDiBCI.png)

```html
<div id="app">
    <!-- <post :text="text"></post> -->
    <post v-for="text in posts" :text='text'></post>
</div>

<script>
    new Vue({
        el: '#app',
        data:{
            posts:[
                'Hello',
                'World',
            ],
        },
        components: {
            post: {
                props: ['text'],
                template: '<div>{{text}}</div>'
            },
        },
    });
</script>
```

陣列中包含物件，也能用綁定方式來渲染
![](https://i.imgur.com/Xi3tlNQ.png)

```html
<div id="app">
    <post v-for="post in posts" :text="post.text" :author="post.author"></post>
</div>

<script>
    new Vue({
        el: '#app',
        data:{
            posts: [
                {text: 'Hello', author: 'Jason'},
                {text: 'World', author: 'Mike'},
            ],
        },
        components: {
            post: {
                props: ['text', 'author'],
                template: '<div>{{author}}:{{text}}</div>'
            },
        },
    });
</script>
```

當要把一個物件其中的屬性，全部指定給 component 作為屬性時，除了一個一個綁定以外，還可以有更快速的方式。
`v-bind="post"`
在上面例子就等於分別綁定`:text="post.text"`，`:author="post.author"`

動態綁定後，會把數值認為”數字“，如果不是動態綁定就會是字串，如果綁定的是 text ，指的就是 text 的變數。

### 單向數據流

Vue 組件的 props 是單向的，也就是'上層傳給下層'。
用 count 的例子來說明：

下層變動的時候，上層則不會改變。
![](https://i.imgur.com/CsSrvO1.png)

上層會往下層傳，下層會被上層重新計算的數值蓋過。
![](https://i.imgur.com/eCFU1nt.png)

```html
<div id="app">
    <!-- 上層 -->
    <h1>{{count}}</h1>
    <button @click="count+=1">+</button>
    <!-- 下層 -->
    <counter :count="count"></counter>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            count: 10,
        },
        // 組件是下層
        components:{
            counter:{
                props:['count'],
                template:`
                <div>
                    <h2>{{count}}</h2>
                    <button @click="count+=1">+</button>
                </div>
                `
            },
        },
    })
</script>
```

如果要用下層改動數值，但不想讓上層蓋過下層，可以在下層的組件中修改 prop 屬性，在組件多指定一個 count 。
把上下層的 count 分開指定。

```html
<div id="app">
    <!-- 上層 -->
    <h1>{{count}}</h1>
    <button @click="count+=1">+</button>

    <!-- 下層 -->
    <counter :start="count"></counter>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            count: 10,
        },
        // 組件是下層
        components:{
            counter:{
                props:['start'],
                data(){
                    return {
                        count: this.start,
                    };
                },
                template:`
                <div>
                    <h2>{{count}}</h2>
                    <button @click="count+=1">+</button>
                </div>
                `
            },
        },
    })
</script>
```

另外，如果下層要對上層傳來的數值做計算，利用computed來計算。
![](https://i.imgur.com/PJGY7DW.png)

```html
<div id="app">
    <!-- 上層 -->
    <h1>{{count}}</h1>
    <button @click="count+=1">+</button>
    <!-- 下層 -->
    <counter :count="count"></counter>

</div>

<script>
    new Vue({
        el: '#app',
        data: {
            count: 10,
        },
        // 組件是下層
        components:{
            counter:{
                props:['count'],
                computed:{
                    countDoubled(){
                        return this.count * 2;
                    },
                },
                template:`
                <div>
                    <h2>{{countDoubled}}</h2>
                </div>
                `
            },
        },
    })
</script>
```

### Prop 驗證

當寫組件之後，要跟別人協作或是組件越來越多的時候，需要描述用到哪些 props、型態等等。

用例子來說明，初始數值設定成字串，但需要的是數字。這邊就需要做檢驗，如果設定到字串會有反應。

把 props 從陣列改為物件，並使用 JavaScript 的資料型態給定 number ，就可以進行驗證，如果型態錯誤則會在 console 中顯示。但引入的 vue cdn 如果是 min.js ，壓縮過的 libraay 則不會在 console 中顯示錯誤。
min 版本是產品會使用的（壓縮過），一般版本則是開發中在使用的。
![](https://i.imgur.com/cL4mgXS.png)

```html
<div id="app">
    <counter start="10"></counter>
</div>

<script>
    Vue.component('counter', {
        props:{
            start: Number, // JS中的資料型態給定，如果異常會在console.log中顯示
        },
        data(){
            return {
                count: this.start,
            };
        },
        template:`
            <div>
                <h1>{{count}}</h1>
                <button @click="count+=1">+</button>
            </div>
        `,
    }),
    new Vue({
        el:'#app',
    })
</script>
```

例子中屬性的 start 也可以是物件，內容也可以給定預設值。
預設值也可以用函式表示，用函式來獲取預設值。

```html
<div id="app">
    <counter></counter>
</div>

<script>
    Vue.component('counter', {
        props:{
            // 也可以是物件
            start:{
                type: Number,
                // default: 0, //一開始不給值這邊也可以給預設值
                // default也可以是函式
                default(){
                    return app.getSetting() // 舉例，用function來獲取值
                },
            }
        },
        data(){
            return {
                count: this.start,
            };
        },
        template:`
            <div>
                <h1>{{count}}</h1>
                <button @click="count+=1">+</button>
            </div>
        `,
    }),
    new Vue({
        el:'#app',
    })
</script>
```

檢驗輸入的值: `validator`，這個參數是在檢驗輸入或給定的值是不是符合條件。比對輸入的值後會回傳 true or false。
    
```html
<div id="app">
    <counter :start="10"></counter>
</div>

<script>
    Vue.component('counter', {
        props:{
            // 可以是物件
            start:{
                type: Number,
                default: 0, //一開始不給值這邊也可以給預設值
                validator(value){
                    return value <= 10;
                    // 比對輸入的值後會回傳true or false
                }
            }
        },
        data(){
            return {
                count: this.start,
            };
        },
        template:`
            <div>
                <h1>{{count}}</h1>
                <button @click="count+=1">+</button>
            </div>
        `,
    }),
    new Vue({
        el:'#app',
    })
</script>
```

**props的檢驗是在組件建立之前**

所以 validator 裡面不會有 this.data 或是 this.myMethod，這些都不會有用，因為檢驗是在組件建立之前。