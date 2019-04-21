---
title: "[Bootstrap] - 切版練習-Blogen"
date: 2019-03-05 14:18:43
tags:
  - bootstrap
category:
  - project
---

## Demo

切版練習-Blogen - [Demo](https://orow.github.io/MyProjects/BootstrapWith5Projects/Blogen-Practice/index.html)

## 1. Introduction 網頁介紹

此版面是 blog 擁有者的管理後台，主要是用 table 來顯示。
不過網頁是用來切版練習，大多數功能是沒有的。

有四個檢視頁面： Dashboard, Posts, Categories, Users。還有 login 頁面，使用者的 profile、setting 等操作頁面。Posts, Categories, Users 都是用不同 title 來顯示 blog 文章，這三個頁面也新增了 serach bar。

Dashborad 中則有`Add Post`，裡面有用`select`、`coustom file`來選則上傳檔案、`CKEditor`所見即所得套件。
Profile 設定中與 add post 類似，也有用到所見即所得套件

## 2. Layout Notes

### 2.1 Dashboard

Navbar 跟先前方式沒有太大不同，不過這個版面的 navbar 左右都有 item，所以需要兩個 ul 來包其中的 li，要把第二個 ul 靠右的話則需要用到`class="ml-auto"`，
![](https://i.imgur.com/X6ERVfo.png)

在右邊的 item 中有 dropdown 的效果
![](https://i.imgur.com/HLHRIYm.png)

Dropdown 在 bootstrap 使用方式直接參考程式碼

```html
<li class="nav-item dropdown mr-3">
  <a href="#" class="nav-link dropdown-toggle" data-toggle="dropdown">
    <i class="fas fa-user"></i> Welcome QQ
  </a>
  <div class="dropdown-menu">
    <a href="profile.html" class="dropdown-item">
      <i class="fas fa-user-circle"></i> Profiles
    </a>
    <a href="setting.html" class="dropdown-item">
      <i class="fas fa-cog"></i> Setting
    </a>
  </div>
</li>
```

Header 區塊的版面用了 col-6 呈現目前的位置，三個 Action 則用了 col-3 切成四等份，來佔了其中三個位置。
![](https://i.imgur.com/Yfk5UC4.png)

個別 action 都用到 modal 的效果
Add post
這邊用了一般包含 label 與 input 的 form-group，也用到 select 選則，上傳 custom-file，最後則是所見即所得套件 CKEditor。
![](https://i.imgur.com/DuehURV.png)

```html
<div class="modal-body">
  <form>
    <div class="form-group">
      <label for="title">Title</label>
      <input type="text" class="form-control" />
    </div>
    <div class="form-group">
      <label for="category">Category</label>
      <select class="form-control">
        <option value="">Web Development</option>
        <option value="">Tech Gadgets</option>
        <option value="">Business</option>
        <option value="">Health & Wellness</option>
      </select>
    </div>
    <div class="form-group">
      <label for="image">Upload Image</label>
      <div class="custom-file">
        <label for="image" class="custom-file-label">Choose File</label>
        <input type="file" id="image" class="custom-file-input" />
        <small class="form-text text-muted">Max Size 3mb</small>
      </div>
    </div>
    <div class="form-group">
      <label for="body">Body</label>
      <!--   注意name名字為CKEditor-function中所需   -->
      <textarea name="editor1" class="form-control"></textarea>
    </div>
  </form>
</div>
```

What you see is what you get 所見即所得套件[CKEditor](https://cdn.ckeditor.com/)
JavaScript code:

```js
// CK Editor - What you see is what you get

CKEDITOR.replace("editor1");
```

Add Categories
一般的 form-group 包了 label 與 input。
![](https://i.imgur.com/VRs3gSx.png)

Add User
與 categories 類似，這邊有四組。
![](https://i.imgur.com/dWk6ANm.png)

table 區塊左邊是表格，右邊則是 card 呈現，左邊表格用了`class="table-striped"`來讓表格行程線斑馬紋交錯。
![](https://i.imgur.com/MlczDF1.png)

### 2.2 Posts

Posts 頁面中大致上沒有太大差別，navbar 相同，Search section 中用 seach bar 呈現。
靠右的方式用上了`class="col-6 ml-auto"`，整個 search bar 則是用`class="input-group"`的方式，後面的 button 則會用 div `class="input-append"`包起來，這樣 button 才會接在前面 input 的後面。
![](https://i.imgur.com/8T5DT5z.png)

最下面則是用了頁面 pagination，此功能尚未連結到真的頁面切換。
![](https://i.imgur.com/3wh45Bi.png)

```html
<nav class="ml-4">
  <ul class="pagination">
    <li class="page-item disabled">
      <a class="page-link" href="#">Previous</a>
    </li>
    <li class="page-item active">
      <a class="page-link" href="#">1</a>
    </li>
    <li class="page-item">
      <a class="page-link" href="#">2</a>
    </li>
    <li class="page-item">
      <a class="page-link" href="#">3</a>
    </li>
    <li class="page-item">
      <a class="page-link" href="#">Next</a>
    </li>
  </ul>
</nav>
```

### 2.3 Categories & Users

這個兩個頁面跟 Posts 相同，把 pagination 拿掉，表格內容少了一個 column。
![](https://i.imgur.com/jtQFHyv.png)

![](https://i.imgur.com/CbssgOy.png)

### 2.4 Details

在 table 中可以點擊`details`button 連結的這個頁面。
![](https://i.imgur.com/aWk8LQn.png)

這個頁面跟前面的 Add Post modal 內容相同，截圖表示：
![](https://i.imgur.com/DbWpz5Z.png)

### 2.4 Setting

整個內容主要部分用 card 呈現，這邊的 form 用了`fieldset`搭配`legend`，讓無障礙能夠友善的閱讀。
主要用`radio` button呈現，注意 name 的屬性內容要一致，才不能複選。
![](https://i.imgur.com/Iyi07hM.png)

其中一個fieldset的程式碼：

```html
<fieldset class="form-group">
  <legend>Allow User Registration</legend>
  <div class="form-check">
    <label class="form-check-label">
      <input type="radio" class="form-check-input" name="yes-no" value="Yes" checked>
      Yes
    </label>
  </div>
  <div class="form-check">
    <label class="form-check-label">
      <input type="radio" class="form-check-input" name="yes-no" value="No">
      No
    </label>
  </div>
</fieldset>
```

### 2.5 Profile

相比details頁面多了一部份有照片跟button的區塊。
![](https://i.imgur.com/uJ6wvku.png)

### 2.6 Login

登入頁面則把nabar全部item都拿掉，內容只有簡單的card包一個form，裡面則是一般的label+input呈現。
![](https://i.imgur.com/PdRfcLo.png)


## 注意事項

- Dropdown
- Custom-file：上傳檔案
- CKEditor：所見即所得
這三項是第一次使用，需要特別注意。