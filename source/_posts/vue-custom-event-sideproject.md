---
title: "[Vue] - （九）自訂事件 Custom Events-實作"
date: 2019-08-30 11:30:00
tags:
  - Vue
categories:
  - project
---

本篇主要是透過線上課程：`HiSKIO`、及網路上搜尋資源所學習的。

## 實作－鄉鎮市區下拉選單

分為兩個下拉選單，第一個下拉選單是選擇縣市，第二個選單則是區域。
選擇後要顯示選擇到的縣市跟區域並帶有郵遞區號。

![](https://i.imgur.com/SbbV1w0.png)

組件命名，如果是用 template ，不是用 html 的話，是不限於大小寫限制的。

用 select 來選擇，Vue 實例中的 template 模板直接用大寫組件名稱`Select`。
V-model 綁定 cityIndex （等於 select 中的 value ，所以在下層`Select`組件中 props 有一個 value 以已經綁定這個值）， v-bind 綁定 options="cities"，這裡的 cities 是 data 中的 cities ，這邊用 computed 來計算並回傳宣告的 cities 物件。

下層 Select 組件中，要有 props 的屬性 value 跟 options 以外， template 中 select v-model 這邊如果是綁定 value 的話，是無法透過 select 後把 value 傳到上層，需要透過 computed 來計算 並用 set 來發出 $emit 事件。

- 組件的 v-model 不能修改 this 的 props （但可以透過 computed 來計算）
computed 用物件表達並用 index 命名後，用 get 取得目前 value ， set 則把目前值用 $emit('input') 的上層去。
上層則是 v-model 的 cityIndex 接收到值。

再來在上層 template 新增一組區域的 Select ，並在 computed 中去執行 areas 的計算獲取宣告的陣列中的區域， zip 則是獲取郵遞區號。
這邊會遇到如果切換的時候，不同 city 在切換時，會因為有不同 index 的 area 導致可能會顯示異常，這邊會用 watch 來偵聽，一旦 cityIndex 切換的時候就把 areaIndex 歸零。

```html
<div id="app"></div>
    
<script>
    const cities = [{
            name: '台北市',
            areas: [{
                    name: '中正區',
                    zip: '100'
                },
                {
                    name: '大同區',
                    zip: '103'
                },
                {
                    name: '中山區',
                    zip: '104'
                },
                {
                    name: '松山區',
                    zip: '105'
                },
                {
                    name: '信義區',
                    zip: '110'
                },
                {
                    name: '南港區',
                    zip: '115'
                },
            ],
        },
        {
            name: '新北市',
            areas: [{
                    name: '板橋區',
                    zip: '220'
                },
                {
                    name: '三重區',
                    zip: '241'
                },
                {
                    name: '永和區',
                    zip: '234'
                },
                {
                    name: '中和區',
                    zip: '235'
                },
                {
                    name: '新店區',
                    zip: '231'
                },
                {
                    name: '新莊區',
                    zip: '242'
                },
            ],
        },
        {
            name: '新竹市',
            areas: [{
                name: '新竹市',
                zip: '300',
            }],
        },
        {
            name: '彰化縣',
            areas: [{
                    name: '彰化市',
                    zip: '500'
                },
                {
                    name: '秀水鄉',
                    zip: '504'
                },
                {
                    name: '花壇鄉',
                    zip: '503'
                },
                {
                    name: '鹿港鎮',
                    zip: '505'
                },
                {
                    name: '員林鎮',
                    zip: '510'
                },
                {
                    name: '溪湖鎮',
                    zip: '514'
                },
            ],
        },
    ];
    Vue.component('Select', {
            props: ['value', 'options'],
            computed: {
                index: {
                    get() {
                        return this.value;
                    },
                    set(val) {
                        this.$emit('input', val);
                    },
                },
            },
            // select v-model 不能直接綁定上層傳來的 value ，因為無法改動上層的index，可以用 computed set 來發送事件達成
            template: `
        <select v-model="index">
            <option v-for="(item, index) in options" :value="index">{{item.name}}</option>
        </select>
        `
        }),
        new Vue({
            el: '#app',
            data: {
                cityIndex: 0,
                areaIndex: 0,
            },
            computed: {
                cities() {
                    // return 外層宣告的 cities
                    return cities;
                },
                areas() {
                    return cities[this.cityIndex].areas;
                },
                zip() {
                    return this.areas[this.areaIndex].zip;
                }
            },
            watch: {
                cityIndex() {
                    this.areaIndex = 0;
                },
            },
            template: `
        <div>
            <h1>County</h1>
            {{cityIndex}}
            <Select v-model="cityIndex" :options="cities"></Select>
            <Select v-model="areaIndex" :options="areas"></Select>
            </br>
            {{cityIndex}} - {{areaIndex}} - {{zip}}
        </div>
        `
        })
</script>
```