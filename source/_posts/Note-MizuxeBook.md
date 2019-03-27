---
title: 切版練習-Mizuxe - Bootstrap
date: 2019-02-12 14:18:43
tags: 
    - bootstrap
category: test
---
## Demo
切版練習-MixuzeBook - [Demo](https://orow.github.io/MyProjects/BootstrapWith5Projects/MizuxeBook-Practice/index.html)

## Introduction
1. 網頁概觀
    * 此版面是介紹一本書
    * Navbar有用logo圖片
    * 主要的showcase section
    * 版面有inline-form可以輸入並且subsribe
    * boxes section
    * About section - accordion
    * Author section - hover author, img above the card
    * contact form - 這裡用input-group
    * Footer
2. Navbar有collapse
3. 有inline-block跟input-group的form
4. About secction中有accordion
5. icon用font awesome
6. 版面
    * navbar用sticky - 像是relative  + fixed的效果
    * 背景overlay用綠色加上opacity


## Layout Notes
### Navbar
1. logo新增圖片,給高度,margin-right與文字不要靠太近
2. container字體加大,但logo用h3字體（因為比li的字體大）
3. container上下padding
4. title跟logo要都是在a裡面,inline排列
5. 有透明度後續再設定


### Showcase
1. 包法
    section(含背景img)
    => div.overlay
    => div.container
    => div.row
    => div.col-6(grid) - 左右各半
    
2. 左邊
    先h1=>往下p=>再往下button(包含font awesome)
    
3. 右邊
    一張背景圖,需要給寬與高
    
4. 修飾左邊
    a. col-6 => text center置中
    b. h1顯示字體要再大=>選擇display-2;需要mt;下方需往上,這裡用pt(mt最多5)
    c. button => btn(為一個button);btn-outline-secondary（secondary為灰底白字,加上outline後為相反-白底灰字;btn-lg（按鈕加大）;滑鼠未移過去時字要白色-text-white
    
5. 修飾背景
    a. 最外層（背景圖）需設定position:relative(給下一層overlay-absolute做參考用)
    b. 最外層（背景圖）需設定min-height(Ｑ-底部question區)
    c. 第二層（overlay)設定absolute的position,從top/left(=0),height/width（100%)都要充滿上一個容器

6. 修飾右邊
    a. img-fulid=>max:width=100%, height=auto

### Newsletter
1. 包法
    section（背景顏色）
    =>container-上下padding(py-5)
    =>row
    =>col-4（3部分平均）
    
2. 前兩個input類似（name & email)
    用form-control, 大一點用form-control-lg
    ref:![](https://i.imgur.com/4CdBF9k.png)
    
3. 後一個為button
    用btn,btn-lg,btn-block
    ref:
    ![](https://i.imgur.com/z847M4c.png)
    沒有用primary or secondary,因為底色不同

### Boxes
1. 包法
    section上下padding(py-5)
    =>container
    =>row
    =>col-3（4部分平均）
    =>每一部分都用card
    
2. card => card-body =>(內容)title用h3,敘述用p 
3. 背景與文字顏色,置中要調整
ref: https://getbootstrap.com/docs/4.2/components/card/

### About/Why section
1. 包法
    section-一樣上下padding(py-5),底色淺灰bg-light
    =>container
    =>row
    =>col - 這邊不需要分幾部分,就only one
    下面內容主要分上下兩大部分
    =>info-header
    內容h1(mb-5,這邊用mb是因為有個底線)->p(pb-3)
    =>accordion
    內容accordion => card(3個都各用一個card)
    card中兩部份
    =>card-header
    內容用h5（mb-0,因h5有mb)包,a連結加上font-awesome
    =>collapse
    內容card-body
for instance:
![](https://i.imgur.com/b9d9KTE.png)

### Authors
1. 包法
    section-一樣上下padding(py-5)
    =>container
    =>上下兩個row
    =>上row單一col
    =>下面row的col分4部分,所以用col-3
    
2. 上面row單一col
    =>info-header(mb-5)
    =>h1(pb-3),p
    
3. 下面row,4個col-3
    =>col-3
    =>card
    =>card-body
    =>img(img-fuild,rounded-circle, w-50, mb-3)
    =>h3,h5(text-muted),p
    =>有3個font-awesome,用d-flex(justify-content-center)包
    =>個別font-awesome用個別div包(有a)
![](https://i.imgur.com/ZO8RwTy.png)
### Contact
1. 包法
    section(淺色bg-light,py-5)
    =>container
    =>row
    =>col-9,col-3（左邊col-9,右邊col-3)
    =>col-9與col-3的部分下面分別說明
    
2. col-9
    h3,p(.lead),form(裡面有3個div,1個button)
    *div部分
    =>div(input-group,input-group-lg,mb-3)
    =>input-group-prepend(要在input中加font-awesome)
    =>有icon也有input所以要有“input-group-text”
    =>第三個div,input要改成textarea(form-control, style="height:187px;")
    *button的部分
    =>button(btn,btn-block,btn-lg)
    
3. col-3
    div(col-3,align-self-center)要垂直置中
![](https://i.imgur.com/TUTIVVq.png)
    

### Footer
1. 包法
    section
    =>container
    =>row
    =>col-6(這裡看起來文字在右邊的中間,左邊是空白所以除了col-6還要ml-auto)
    =>p(lead)
![](https://i.imgur.com/4Z3ySTw.png)


 
### Question 
1. Accordion的包法要多多練習-collapse
2. Collapse如果有多個,如何寫出個別點開,不會同時只能有一個collapse出現?
    Q. 找範例
3. Navbar固定在網頁最上面,fixed要加一個相同長寬的div(尚未試過) or 用margin-top處理(在下一個div);目前用sticky