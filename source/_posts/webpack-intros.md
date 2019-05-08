---
title: "[Webpack] - 認識 webpack & babel"
date: 2019-05-27 22:00:00
tags:
  - webpack
  - babel
categories:
  - webpack
---

## Webpack

[官網](https://webpack.js.org/)

下面圖示就說明了webpack在做什麼的，左邊是各種工具、第三方前處理等等的方式，透過webpack轉譯成瀏覽器能夠支援、讀得懂的內容。
![](https://i.imgur.com/F50uEw5.png)


### 為什麼要用webpack

參考來源 - [為什麼我們需要webpack](https://www.youtube.com/watch?v=OLPuNJwKTsA)

因為現在前端有很多新的技術，但是瀏覽器並沒有完全支援，這時候就可以透過webpack來編譯成瀏覽器所看的懂的html、css、javascript等等。
![](https://i.imgur.com/AsBlugf.png)

這樣就可以學習、使用新技術，來用更方便，有效的方式來開發。

1. 編譯新技術來讓瀏覽器可以支援
2. webpack也可編譯sass、壓縮圖片等等。
3. 可以統一多人的開發環境，這樣在該專案中，引入哪些套件、用哪些方法、版本號等等，都可以用webpack來維持。


### 例子

多個js檔案，透過webpack來轉成一個js檔。

以前，js拆開、一次一次引入
![](https://i.imgur.com/b24L1Or.png)

但在script載入的時候是非同步的載入，沒辦法確定哪一隻js先引入

現在
![](https://i.imgur.com/0fg1UGB.png)

webpack把所有plugin（bootstrap, jQuery等）編成vendor.js的檔案，然後所寫的內容只會有index.js檔案，確保document.reday只會有一個等等。
這樣可以確保程式的進入點只會一個，確保程式的flow正確性，以減少程式開發的錯誤跟問題的發生。


webpack是建立在node.js的開發環境上。
![](https://i.imgur.com/xOv6DXE.png)

- 安裝node.js前要先安裝nvm，node版本管裡
- 安裝node後也會安裝npm，node套件管理


### webpack超入門

參考來源 - [Webpack 前端自動化開發超入門](https://www.youtube.com/watch?v=vyI-Ko6fvKU)

sass, pug , gulp第三方的前處理工具，或是react框架等，這些可以用不一樣的工具來編譯，但用webpack可以統一編譯轉換。


## Webpack + Babel

使用webpack前要先有`webpack.config.js`
![](https://i.imgur.com/gI7qrXW.png)

[官網說明](https://webpack.js.org/guides/getting-started/)：安裝順序

```js
mkdir webpack-demo
cd webpack-demo
npm init -y
npm install webpack --save-dev
npm install webpack-cli --save-dev
```

`npm init-y`初始化會產生`package.json`檔案，裡面包含了使用到的工具版本、指令等等。

`npm install webpack --save-dev`跟`npm install webpack-cli --save-dev`後會產生`package-lock.json`跟`node_modules`。

node_modules內容就是使用到的第三方套件，如果要專案複製的話，通常不會放這個資料夾，因為可以透過npm install裝回來。

npm install後面的`--save-dev`會把安裝的版本寫進`package.json`
![](https://i.imgur.com/aSIWszm.png)


webpack一開始最重要的就是`entry`跟`output`，entry就是自己寫的js內容，output就是轉譯後丟出去的路徑跟名稱。


webpack.config.js

```js
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  }
};
```

require是node.js語法，上面寫法是從node.js把path這個模組抓進webpack來使用。
`path: path.resolve(__dirname, 'dist')`中的resolve就是把相對路徑轉換為絕對路徑。影片中在terminal中輸入~~webpack~~就會在同一層路徑產生dist資料夾，裡面有bundle.js檔案。

    目前官網上說明指令為npx webpack


### Babel-loader

但是如果要轉換高版本javascript語法到ES5的話，就需要掛載babel來轉。

[Babel官網](https://webpack.js.org/loaders/babel-loader/)
[Babel-Github](https://github.com/babel/babel-loader)

- babel-loader

    要透過webpack轉譯前，要先讓webpack可以讀懂ES6或是更高版本的語法，babel-loader的作用就是在此。
    
    安裝指令 `npm install -D babel-loader @babel/core @babel/preset-env webpack`
    這邊沒有加上`--save-dev`，是因為用了`-D`這個縮寫，結果是相同的，會把版本存進package.json
    
安裝babel-loader後要對loader設定進行設定。
要在webpack.config.js中加入module。

```js
module: {
  rules: [
    {
      test: /\.m?js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-env']
        }
      }
    }
  ]
}
```

在安裝完babel-loader跟加入module後，再執行一次`npx webpack`就會轉為ES5語法了。


### Mike補充

在webpack中可以設定npm script

mike文章中有例子
在package.json設定新增script，像下面例子這樣就可以透過npm指令來執行webpack

```json
 "scripts": {
    "watch": "webpack --mode development --watch",
    "start": "webpack --mode development",
    "deploy": "webpack --mode production"
    },
```

指令

```
$npm run watch
$npm run start
$npm run deploy
```


在一開始的例子沒有使用這個script前，在執行npx webpack會出現警告
![](https://i.imgur.com/OQCmpWv.png)

意思是沒有設定mode，代表每一次都是production

- development: 不壓縮，可讀性較高
- production: 檔案內容都會壓縮成一行，減少內容，提高執行效率

如果想要持續執行，就可以加上 --watch指令，只要修改entry的js來源的內容，一存檔就會自動執行webpack。


## 模組化


用一開始提到的例子當範例
![](https://i.imgur.com/b24L1Or.png)

共有三個檔案index.js、login.js、menu.js。

官網上也有說明
![](https://i.imgur.com/ZN62GVr.png)

在menu.js檔案中寫

```js
export default function menu() {
  console.log("menu");
}
```

html檔案會引用的index.js中寫

```js
// import menu.js
import test from './menu.js';
window.onload = function(){
    test();
}
```

這樣在網頁一直行完就會去執行menu.js檔案裡面的menu函式，就會得到console.log('menu')的結果
![](https://i.imgur.com/qjGjiCS.png)

login也是一樣的做法

```js
export default function Login(){
    console.log('login');
}
```

html檔案會引用的index.js中寫

```js
// import menu.js & login.js，.js省略可運作
import test from './menu.js';
import testLogin from './login.js'
window.onload = function(){
    test();
    testLogin();
}
```

得到
![](https://i.imgur.com/w4gClzv.png)