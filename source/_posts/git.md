---
title: Github / Git 基礎指令
date: 2019-01-17 00:24:43
tags: 
    - html
    - css
---


# Github / iTerm2 / SourceTree - macOS

### Installation steps
1. Git
![](https://i.imgur.com/0kHoO9k.png)
*需要＾＋點擊後選擇open with installer(default)
3. iTerm2 (Mac有內建的terminal)
![](https://i.imgur.com/Tfp6O7U.png)
3. SourceTree
![](https://i.imgur.com/Yw3XZ3a.png)



### ssh key產生 
ssh是Secure SHell protocl 的縮寫
ref: https://blog.alantsai.net/posts/2015/09/use-ssh-in-windows-for-github
1. iTerm2 下輸入
```
ssh-keygen -t rsa -C "your_email@example.com"
```
> 已註冊Github帳號,輸入在github上的郵件

預設產生路徑：~user_name/.ssh/id_rsa (隱藏)
ref : https://www.jianshu.com/p/9317a927e844

2. 產生後要把公鑰上傳到github

### 從github複製專案
1. 從Github上開啟repositories
2. 選擇要clone的專案download
![](https://i.imgur.com/dZZLqdZ.png)
    a. 可用指令執行
        =>用iTerm 輸入 git clone "github上提供的位址"
    b. 可download整個專案
        =>用SourceTree開啟
ref: https://gogojimmy.net/2012/01/17/how-to-use-git-1-git-basic/
3. clone到本機端後開啟SourceTree到該路徑scan即可在本機端看到該專案


### 透過SourceTree remote連動 - 建議為專案擁有者
另一方式不clone,直接在SourceTree設定中新增github帳號
此方式建議為專案擁有者本身,是用remote方式
![](https://i.imgur.com/kRS20RW.png)


### Git 基礎指令
1. 建立新專案
        * 在github上新建立repository
2. 初始化git專案
    * 先把路徑只到該專案資料夾
        `cd your_project_name`

    * 初始化
        `git init`

3. 第一次要remote這個repository需要輸入以下指令
    `git remote add origin git@github.com:github帳號/repository名稱.git`
    
4. 修改檔案後常用指令
    1. git add
        `git add index.html(檔案名稱)`

    2. git status
        `git status`
        * 可以確認狀態
        * 尚未add的檔案顯是untracked file,且顯示紅色
        * add過的檔案則顯示綠色並顯示change to be committed
        
    3. git commit
        `git commit -m "first commit"`
        * ""中為使用者要給這次commit的內容簡短標注

    4. git push
        `git push`
        * 第一次要使用`git push -u origin master`, 讓他變成default, 之後只需要使用`git push`即可

    5. git log
        `git log`
        * 用來確認剛剛做的事情, 也可以確認commit有沒有成功

    6. git checkout
        `git checkout branch_name`
        * 可以切換目前位置到哪一個branch
        
### 使用github展示靜態網頁

1. 建立branch
    * 名稱需為gh-pages
    * 指令為`git branch gh-pages`

2. push上github
    * 指令`git push -u origin gh-pages`
    * 之後只要`git push`即可

3. 到[github_name].github.io/[repository_name]/查看，即可看到網頁
    **首頁必須要命名為index.html**