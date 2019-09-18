---
title: "[Vue] - （十一）路由配置 Vue-Router（下）"
date: 2019-09-18 18:00:00
tags:
  - Vue
categories:
  - Vue
---

本篇主要是透過線上課程：`HiSKIO`、[官方文件](https://vuejs.org/v2/guide/single-file-components.html)及網路上搜尋資源所學習的。

## 路由配置（Vue-Router）

### 具名路由與具名視圖

Named Routes & Named Views
不想要把 router-link 的路徑寫很長，可以把組件的路徑去做命名。

About.vue

```html
<template>
  <div>
    <h1>About</h1>
    <!-- <router-link to="/about/us">AboutUs</router-link> -->

    <!-- 具名路由 Named Routes -->
    <router-link :to="{name:'us'}">AUs</router-link>
    <router-link to="/about/family">AboutFamily</router-link>
    <router-view/>
  </div>
</template>
```

rotuer.js

```js
// 把下面這個組件AboutUs命名為Us，在template中的to的路徑改為{name: 'us'}，to改為:to動態綁定
{ path: "us", name: 'us', component: AboutUs },
```

note: 如果命名的名稱有相同的，只會抓取第一個符合條件的組件帶入。

除了 name 命名外，也可以給 params 來命名。

About.vue

```html
<template>
  <div>
    <h1>About</h1>
    <!-- <router-link to="/about/us">AboutUs</router-link> -->
    <!-- <router-link to="/about/family">AboutFamily</router-link> -->

    <!-- 具名路由 Named Routes -->
    <router-link :to="{name:'us'}">Us</router-link>
    <router-link :to="{name: 'prods', params:{id:10}}">Mask</router-link>
    <router-view/>
  </div>
</template>
```

rotuer.js

```js
{ path: "/products/:id?",
  // 具名路由
  name: 'prods',
  component: Products,
},
```

![](https://i.imgur.com/bX26sbN.png)

![](https://i.imgur.com/KcyqyHB.png)


同時顯示一個以上的組件的話也可以達到。
![](https://i.imgur.com/UggmYiq.png)


在 router.js 中要新增路徑，並把 component 改為複數 components 。

```js
{ path: "both", components: {
  a: AboutUs,
  b: AboutFamily,
}},
```

App.Vue中 的 router-view 進行對應的命名。

```html
<template>
  <div>
    <h1>About</h1>
    <!-- 具名路由 兩個都顯示 -->
    <router-link to="/about/us">AboutUs</router-link>
    <router-link to="/about/family">AboutFamily</router-link>
    <router-link to="/about/both">Both</router-link>
    <!-- <router-view/> -->
    <router-view name="a"/>
    <router-view name="b"/>
  </div>
</template>
```

但這樣設定的話就 About Us 跟 AboutFamily 就不見了，因為 router-view 改成 name=a 跟 b 了。

這邊可以把 name 改成 default，因為`<router-view name="default"/>` 跟`<router-view/>`是相同的。

改完後如下

About.vue

```html
<template>
  <div>
    <h1>About</h1>
    <!-- <router-link to="/about/us">AboutUs</router-link> -->
    <!-- <router-link to="/about/family">AboutFamily</router-link> -->

    <!-- 具名路由 Named Routes -->
    <router-link :to="{name:'us'}">Us</router-link>
    <router-link :to="{name: 'prods', params:{id:10}}">Mask</router-link>

    <!-- 具名路由 兩個都顯示 -->
    <router-link to="/about/both">Both</router-link>
    <!-- <router-view/> -->

    <!-- name="default"是預設 -->
    <router-view name="default"/>
    <router-view name="another"/>
  </div>
</template>
```

router.js

```js
{ path: "both", components: {
  default: AboutUs,
  another: AboutFamily,
}},
```

![](https://i.imgur.com/aGmPVUM.png)

### 轉址與別名

Redirect & Alias
redirect 路徑是會變動（跳轉），alias 別名則路徑不會改變。

**轉址- Redirect**

可以接受三種值

- 字串
- 物件
- 函式

如果路徑是是輸入 /aboutus 自動轉到 /about/us 的行為。

router.js

```js
{
  path: 'aboutus',
  redirect: '/about/us',
},
```

如果是要隨便亂打也可以轉址到想要的路徑的話，把`path設定為*`即可。

但`path:'*'`的設定建議放最後，這樣前面的路徑設定有匹配的會先符合，才不會轉指到 redirect 指定的位置。

```js
// 轉址
{
  // path: 'aboutus',
  path: '*',
  redirect: '/about/us',
},
```

物件

![](https://i.imgur.com/DdycL2R.png)

```js
export default new VueRouter({
  routes: [
    {
      path: '/',
      component: App,
      children: [
        // nested routes
        {
          path: "about",
          component: About,
          children: [
            { path: "", component: AboutAllOfUs },
            { path: "us", name:'us', component: AboutUs },
            { path: "family", name:'family', component: AboutFamily },
            { path: "both", components: {
              default: AboutUs,
              another: AboutFamily,
            }},
          ],
        },
        { 
          path: "/products/:id?",
          // 具名路由
          name: 'prods',
          component: Products,
        },
        // 轉址
        {
          // path: 'aboutus',
          path: '*',  // *指的是任意路徑
          // redirect: '/about/us',
          redirect: {name: 'family'},
        },
      ],
    },
  ],
});

```

函式

```js
<script>
// 轉址
{
  // path: 'aboutus',
  path: '*', 
  // redirect-字串
  // redirect: '/about/us',

  // redirect-物件
  // redirect: {name: 'family'},

  // redirect-函式
  redirect: (from) => {
    // return '/about',
    return { name: 'family'};
  },
},
</script>
```

**別名- Alias**

上面說到的轉址，path 改為 story，redirect 改為 about，實際上路徑輸入 story 後會轉為 about。

```js
{
    path: '/story',
    redirect: '/about',
},
```

![](https://i.imgur.com/YYDuZts.png)

但如果是用 alias 別名的方式，路徑是不會變動，但顯示會是相同的。
在 path 是`about`的這層中用 alias 。

```js
export default new VueRouter({
  routes: [
    {
      path: '/',
      component: App,
      children: [
        // nested routes
        {
          path: "about",
          // Alias別名
          alias: "story",
          component: About,
          children: [...],
        },
      ],
    },
  ],
});
```

![](https://i.imgur.com/ZcmDADb.png)

別名也可以接受陣列的值

```js
{ path: "both",
  // Alias別名也可以是陣列
  alias: ['/2', '2', '3'],
  components: {
  default: AboutUs,
  another: AboutFamily,
}},
```

![](https://i.imgur.com/JcjPNgA.png)

![](https://i.imgur.com/PjEDV0o.png)

### 組件的props Passing Props to Route Components

在 path 中用`:`加上名字來指定 params 的時候，在對應的組件中可以用`this.$route.params.id`來存取。這個`this.$params`是在使用 vue-router 的時候，如果在 route 中指定該 component，render 出來後，實例就會帶有`$route`的屬性，是透過 vue-router 注入到到組件中的，所以運作時要透過`this.$route`。

但如果在一般不是透過 router render 出來的組件想要使用，就要透過一些 props 設定來使用。

props 可以是三種
- booleans
- 物件
- 函式

在要先該組件中去註冊`props:['id']`，然後在 router 該組件中去設定`props: true`，那麼路徑設定的 params 會當成組件的 props 來傳遞，組件中會認為是上層傳遞來的 props。

router.js

```js
{ 
  path: "/products/:id?",
  // 讓props為true
  props: true,
  // 具名路由
  name: 'prods',
  component: Products,
},
```

Products.vue

```js
<script>
// dynamic route matching
// products帶內容
const products = {
  7: "Snorkel",
  10: "Mask",
  15: "Fins"
};

export default {
  // 註冊id的屬性
  props:['id'],

  // 註冊id的props後，computed就不需要
  // computed: {
    // products帶內容
    // id() {
      // return products[this.$route.params.id];
    // },
  // },
};
</script>
```

另一種用法，props 也可是物件，假設也不需要 router 傳 params，只需要指定 props，可以用物件的方式，直接在 props 指定名稱，這個組件 render 出來就會帶有固定 id。
![](https://i.imgur.com/JW2yIzH.png)


router.js

```js
{ 
  // path: "/products/:id?",
  // props為true
  // props: true,

  // props指定為物件
  path: "/products",
  props: {id:10},

  // 具名路由
  name: 'prods',
  component: Products,
},
```

props 也可以指定為函式

![](https://i.imgur.com/Q2B3nOH.png)


```js
path: "products",
// props指定為函式
props: ()=> {
    return {
      id: 20,
    }
},
```

 這邊要注意到函式中參數`route`會跟`$this.route`時一樣的東西
 
 ```js
path: "products",
// props指定為函式
props: (route) => {
    return {
      id: 20,
    }
},
```

所以也可以寫成
```js
// 函式也可以寫成
path: "products/:id?",
props: (route)=> {
    return {
    // route = this.$route
      id: route.params.id,
    };
},
```