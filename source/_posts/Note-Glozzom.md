---
title: 切版練習-Glozzom Multi Page- Bootstrap
date: 2019-03-03 14:18:43
tags:
    - bootstrap
category: test
---
## Demo
切版練習-Glozzom Multi Page - [Demo](https://orow.github.io/MyProjects/BootstrapWith5Projects/Glozzom-Practice/index.html)

## 1. Introduction 網頁概觀
    * 此版面有五個頁面： Home, About, Services, Blog, Contact
### 1.1 Home


![](https://i.imgur.com/W3vWLxv.png)

(下面說明皆截圖，並去掉 - 號)
- Navba含collapse
- Showcase - Carousel slider，是整個背景圖片slide，還有indicator, arrow加上h1, p, button
    - arrow要加上slide prev & next的功能
- Home-icon section: font awesome icon and title & p
- Home-header section: 捲動的時候背景圖片會固定，這邊背景圖片用fixed、位置用relative，overlay位置用absolute再給透明度
- Video-section: 這邊用到modal(video)跟iframe，背景圖片也是fixed，也有overlay
- Photo-gallery section: 用到lightbox，照片我用unsplash隨機的照片
- Newsletter section: inline-block的form
- Footer

### 1.2 About

- Navbar: 相同，只有移動active class到該頁面的區塊
- Page header: 背景圖片仍是fixed
- Icon boxes: 含有font awesome的box，用card呈現
- Testimonial: 文字需要slider，文字後面要加上blockquote-footer

### 1.3 Services

與About頁面類似，Services section: 用card呈現
(這邊截圖)
使用的是 Card 元件，方法很簡單，加上以下三個 class 即可，`.card-header.card-body.card-footer`

Accordion: 要分成兩邊，一邊各有三個collapse，每個collapse需要有自己的名稱並用`a herf`來連結

### 1.4 Blog
Blog section:
(這邊截圖顯示)

用card-columns，這裡使用了6個card
這裡的card有兩種
    1. 中有加img, h4, small, p等等
    2. blockquote: p, footer(small > cite)

(這邊也須截圖說明d)

### 1. 5 Contact
Contact section: 左邊為information，右邊為form-group表單

taff section: 四個staff，附上照片


2. icon用font awesome
3. 功能
    * Carousel slider
    * Modal + iframe
    * [lightbox](http://ashleydw.github.io/lightbox/)
    * testimonials slider
    * Accordion


## index.html

### Navbar
1. 包法
    =>nav class="navbar navbar-expand-sm navbar-dark bg-dark"
    =>container
    =>container下含
    ＝> a, button, div class="collapse navbar-collapse"
2. 一般修飾：
    1. 最上面nav的navbar-dark可以讓字為白色
    2. a class="navbar-brand"
    3. button class="navbar-toggler" data-toggle="collapse" data-target="navbarCollapse"

3. div class="collapse navbar-collapse" id="navbarCollapse"
    =>以下含ul class="navbar-nav ml-auto"
    =>li class="nav-item"
    =>li再包a class="nav-link"
    *li共五個,在Home那個的li class多加active來初始化


### Showcase slide
`此範例從sandbox複製過來修改`
1. 包法
    =>section id="showcase"
    =>div id="myCarousel" class="carousel slide" data-ride="carousel"
    =>ol class="carousel-indicators",
    div class="carousel-inner,
    a href="#myCarousel" class="carousel-control-prev" data-slide="prev",
    a href="#myCarousel" class="carousel-control-next" data-slide="next"
2. div class="carousel-inner"
    1. 含三個div class="carousel-item"
    2. carousel-item => img class="d-block img-fluid", div class="carousel-caption d-none d-md-block mb-5"
    3. carousel-caption中
        a. h1 class="display-3"
        b. p class="lead"
        c. button class="btn btn-lg btn-danger（個別按鈕顏色不同)
3. 背景圖片這邊要注意,包著他的要給高度,若高度不是給容器,width無法填滿

### Home icon section
1. 包法
    =>section id
    =>container
    =>row
    =>col-4 * 3
    =>每個col-4要包i,h3,p
2. 說明
    1.section class要加"py-5"
    2.col-4 也要有text-center

### Home header section
1. 包法
    =>section
    =>overlay
    =>container, text-center, text-white
    =>row
    =>col, py-5
2. 背景
    1. 只要no-repeat, fixed(attachment)
    2. 給min-height
    3. position:relative(因為下一層overlay需要absolute)
3. overlay
    1. background: rgba(0,0,0,0.7)
    2. top:o , left: 0
    3. width, height:100%
    4. position: absolute

### Home info section
1. 包法(py-3)
    =>section
    =>container
    =>row
    =>col-6 * 2
    =>左邊col-6(align-self-center)
    1. h3
    2. p
    3. button(btn, btn-outling-danger, btn-lg)
    =>右邊col-6
    1. img(img-fluid)


### Video play
1. 包法
    =>section(text-center)
    =>overlay
    =>container
    =>row
    =>col(py-5)
    =>包含a, i, h1
2. 說明a(modal)
    1. href="#"
    2. class="video text-white"
    3. data-toggle="modal", data-targer="videomodal", data-video=https://www.youtube.com/embed/HnwsG9a5riA"
`embed是內嵌的部分: https://www.youtube.com/embed`
3. video modal
    1. 寫在footer之後
    2. 包法
        =>div class="modal fade", id="videomodal"
        =>div class="modal-dialopg"
        =>div class="modal-content"
        =>div class="modal-body"
        body中含
            1. button
                a. class="close"
                b. data-dismiss="modal"
                c. span包住叉叉符號：&times; = `&times;`
                *這樣才能點擊叉叉後關閉
            2. iframe
                a. allowfullscreen是讓他可以全螢幕
                b. 需增加js menthod

        ```js
        <script>
                // Video Play
                $(function () {
                    // Auto play modal video
                    $(".video").click(function () {
                        var theModal = $(this).data("target"),
                            videoSRC = $(this).attr("data-video"),
                            videoSRCauto = videoSRC +
                            "?modestbranding=1&rel=0&controls=0&showinfo=0&html5=1&autoplay=1";
                        $(theModal + ' iframe').attr('src', videoSRCauto);
                        $(theModal + ' button.close').click(function () {
                            $(theModal + ' iframe').attr('src', videoSRC);
                        });
                    });
                });
            </script>
        ```

### Photos gallery
1. 包法
    =>section
    =>container(text-center,py-5)
    =>h1
    =>p
    =>兩個row(mb-4)
    =>每個row各三個col-4
    =>col中包a
    =>a包img
2. 描述
    1.兩個row上下有間隔,用mb-4
    2.a要lightbox(google搜尋),點擊image要彈出小視窗
    3.img要使用隨機照片(ref:https://source.unsplash.com/),但是後面尺寸不能設一樣不然會同一張
    4.lightbox還要有script(lightbox for bootstrap)

### Newsletter
1. 包法
    =>section(text-center text-white py-5 bg-dark)
    =>container
    =>row
    =>col
    =>h1, p, form
    =>form包含input * 2, button * 1
2. 描述
    1. form是同一行用form-inline;且水平位置中間平均=>用justify-contner-center
    2. 彼此要有間隔用mr-2

### footer
1. 包法
    =>section(id=main-footer),為了背景跟字體顏色
    =>cotainer
    =>row
    =>col
    =>p
2. 描述
    1. p中的日期2019 給的span(id="year")包起來,有用產生日期的js function
        ```js
         // Get the current year for the copyright
                $('#year').text(new Date().getFullYear());
        ```

## About.html

### 前置作業
1. Copy all index.html content to about.html
2. Remove lightbox & video play(link & script tag)
3. Add slick & slick-theme link tag, slick script tag
    ref: CDN: https://cdnjs.com/libraries/slick-carousel
    Js function: http://kenwheeler.github.io/slick/

4. Keep content navbar & footer only

### Page-header
1. 包法
    =>header
    =>container
    =>row
    =>col
    =>h1,p
2. 描述
    1. 背景:給高度, bgd-position:0 -360px, 固定位置
    2. 內容文字padding-top
    3. 看起來都會加border-bottom

### About-section
1. 包法
    =>section
    =>container
    =>row
    =>col-6 * 2
    =>左邊col-6
    =>h1, p, p
    =>右邊col-6
    =>img(random/700x700/?technology)
2. 描述
    1. 左邊col-6
    2. 右邊col-6用img-fluid rounded-circle;還要再margin-top(用負的)

### Icon-boxes
1. 包法
    =>section(text-white text-center py-5)
    =>container
    =>row * 2 (mb-4)
    =>每個row有col-4 * 3
    =>每個col-4再用card包
    =>icon, h3, 文字(這邊不用p,會有margin-bottom)
2. 描述
    1. col-4下的div(card card-body)
    2. icon, h3(沒用card-title), 文字

### Testimonials
1. 包法
    =>section(testimonials)
    =>container
    =>h2
    =>row
    =>col
    =>div(slider)
    =>div * 3(3個testimonials內容)
    =>每個div下包blockquote
    =>blockquote下包p, footer
    =>footer要包cite
2. 描述
    1. section(text-white text-center py-4 bg-dark)
    2. h2在row之前,可能這次slide的不包含h2
    3. col下要有div class="slider"包住
    4. 每一個內容都要有div=>blockquote class="blockquote"再去包p & footer
    5. p要mb-0扣掉margin-bottom
    6. footer(class="blockquote-footer")中還需要包cite(title也要打)
    7. 需要jQuery function


## Services.html

### 前置作業
1. Copy all about.html content to services.html
2. Remove slick & slick-theme link tag, slick
3. Keep content navbar & Page-header & footer

### Service section
1. 包法
    =>section
    =>container
    =>row
    =>col-4 * 3
    =>每個col-4包card
    =>card包card-header, card-body, card-footer
    =>card-header包h3
    =>card-body包h4(card-title), p(card-text), ul(用list-group)
    =>card-footer
    =>ul包li * 4 + a(btn btn-blcok mt-2 btn-danger)
2. 描述
    1. section(py-5)
    2. card(text-center)
    3. ul(list-group)中的li(list-group-item)

### FAQ section
1. 包法
    =>section
    =>container
    =>h1
    =>hr
    =>row
    =>col-6 * 2
    =>每個col-6包Accordion
    =>div(accordion)
    =>div(card)
    =>div(card-header), div(collapse,#collapse編號)
    =>card-header包h5包a(href="#collapse編號" data-parent="#accordion" data-toggle="collapse")
    =>div(collapse, #collapse編號)包card-body
2. 描述
    1. section(py-5, text-white)
    2. h1(text-center)
    3. card-header中的h5(mb-0)
    4. collapse


## blog.html

### 前置作業
1. Copy all services.html content to blog.html
2. Keep content navbar & Page-header & footer

### blog section
1. 包法
    =>section(py-3)
    =>container
    =>row
    =>col
    =>card-column
    =>card * 6
    =>card中(1,2)一組,(3,4)一組,(5,6)一組
    =>以一組來說明
    =>第一組card
    =>div(card)
    =>img(card-img-top img-fluid)
    =>card-body
    =>card-body中包h4(card-title), small(text-muted), hr, p(card-text)
    =>第二組card
    =>div(card p-3)
    =>blockquote(card-blockquote card-body)
    =>p
    =>footer(blockquote-footer)
    =>footer中包small
    =>small中包cite(title)


## contact.html

### 前置作業
1. Copy all blog.html content to contact.html
2. Keep content navbar & Page-header & footer

### contact section
1. 包法
    =>sectino
    =>container
    =>row
    =>col-4,col-8
    1. col-4
        a. card(p-4)
        b. card-body
        c. h4, p, h4, p, h4, p, h4, p
    2. col-8
        A. Orow's Way
        1. card(p-4)
        2. card-body
        3. h3(text-center)
        4. hr
        5. row * 2
        6. 上row包col-6 * 2
        7. 每個col-6 => form-group => input * 2
        8. 下row包col => form-group
        9. 包textarea, button

        B. Brad's Way
        1. card(p-4)
        2. card-body
        3. h3(text-center)
        4. hr
        5. row * 2
        6. 上row包col-6 * 4
        7. 每個col-6 => (form-group => input )* 1
        8. 下row包col-12 * 2(用col的話兩個會同一行)
        9. 一個col-12包form-group => textarea * 1
        10. 另一個col-12包form-group => input * 1
2. 描述
    1. 注意上下尺寸,左邊card用card-title/card-text會沒有mb
    2.  input 要記得用form-control


### staff
1. 包法
    =>section(staff),class(text-center,text-white,bg-dark,py-5)
    =>container
    =>h1
    =>hr
    =>row
    =>col-3 * 4
    =>每個col-3
    1. img(rounded-circle, mb-2)
    2. h4
    3. small
