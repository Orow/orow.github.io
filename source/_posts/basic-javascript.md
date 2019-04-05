---
title: "[JavaScript] - Basic"
date: 2019-02-28 20:00:00
tags:
  - javascript
categories:
  - javascript
---

新手針對網路課程與資料做筆記記錄。
在剛接觸時加深自己印象與記憶點。
網路課程：`freeCodeCamp`, `Udemy`, `eloqument`

## JavaScript 基礎概念

JavaScript 是一門基於原型、函式先行的語言，支援物件導向設計。
白話的說是一種用來呈現網頁動態效果的程式語言，例如：更新內容、控制動畫圖片等

### 概論

以下引用[wiki](https://zh.wikipedia.org/wiki/JavaScript)

    一般來說，完整的JavaScript包括以下幾個部分：
    - ECMAScript，描述了該語言的語法和基本物件
    - 文件物件模型（DOM），描述處理網頁內容的方法和介面
    - 瀏覽器物件模型（BOM），描述與瀏覽器進行互動的方法和介面

    JavaScript的基本特點如下：
    - 是一種解釋性程式語言（程式碼不進行預編譯）。
    - 主要用來向HTML頁面添加互動行為。
    - 可以直接嵌入HTML頁面，但寫成單獨的js檔案有利於結構和行為的分離。

    JavaScript常用來完成以下任務：
    - 嵌入動態文字於HTML頁面
    - 對瀏覽器事件作出回應
    - 讀寫HTML元素
    - 在資料被提交到伺服器之前驗證資料
    - 檢測訪客的瀏覽器資訊
    - 控制cookies，包括建立和修改等

### 動態型別

屬於弱型別，也能說是動態的程式語言，意思是不用特別宣告變數的型別。
程式在運作時，型別會自動轉換，代表可以以不同型別使用同一個變數

### 資料型別

原始型別

- string： 字串
- number： 數字
- boolean：true or false
- null: 只有一個 null 值
- undefined： 未被定義的參數有 undefined 值
- symbol： ES6 定義，個人尚未用到，待後續深入學習

物件型別

- object：在 JavaScript 裡，"物件"是一個擁有自己的屬性、型別、獨立的實體。
  這裡我們以杯子舉例：我們可以從顏色、設計、重量、材質等方面來描述他的屬性。同樣的，我們也可以用各種屬性來描述 JavaScript 中某個物體的特性。
  下為一個物件的例子。

  object={key: value}
  name, ages 是 key ; ‘陳小美’, 22 為 value

  ```js
  ar auntie = {
      name: '陳小美',
      ages: 22,
          bwh: {
          strength: 34,
          agility: 25,
          intelligence: 96
      },
      single: true
  };
  ```

  - [參考來源](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Working_with_Objects)

原始型別是一個值，沒有屬性，且是不可變異(immutable)。
必要時，JavaScript 引擎會將原始別強制轉型為對應的物件型別（除了 null & undefined）。

- [參考來源](https://ithelp.ithome.com.tw/articles/10192598)

```js
var foo = 42; //foo 目前是number
var foo = "bar"; //foo 目前是string
var foo = true; // foo目前是boolean
```

## JavaScript 基本功能介紹

### 運算子(Operators)

一元運算子: 只用到一個運算元

- `++`, `--`等遞增遞減

二元運算子：有兩個運算元（大部分運算子都是二元）
舉例：

- 比較運算子
  ![](https://i.imgur.com/Yf3vaaO.png)

三元運算子

- 條件運算子
  (condition ? statement-if-true : statement-if-false;)
  也就是：條件式 ? 成立傳回值 : 失敗傳回值

運算子眾多，詳細請參考[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators)

### 跳脫字元

特殊字元例如單引號或雙引號在 javascript 中是用來包字串，跳脫字元就是用來將這些字元納入字串中。
在這些字元前都必須加入反斜線
如下圖：
![](https://i.imgur.com/bPWbsMH.png)

### 陣列(Array)

Array 在 javascript 是用來存取數據的工具，可以存放其他物件或是數值。
這邊舉幾個常用例子，詳細可以到 [MDN-Array](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array) 上作查詢

建立 Array

```js
var arr = ["Apple", 2, 3, "Banana"];
```

用 index 取的 array 中的 item

```js
var first = arr[0]; // Apple
var third = arr[2]; // 3
```

push()：從 array 最後加入 item

```js
var newNumber = arr.push("5"); // arr = ['Apple', 2, 3, 'Banana', 5];
```

pop()：移除 array 最後一個 item

```js
var last = arr.pop(); // arr = ['Apple', 2, 3, 'Banana'];
```

shift()：移除 array 最前面一個 item

```js
var first = arr.shift(); // arr = [2, 3, 'Banana'];
```

unshift()：從 array 最前面加入 item

```js
var newFruits = arr.unshift("Watermelon"); // arr = ['Watermelon', 2, 3, 'Banana'];
```

splice(pos, index)：移除從 pos 起的幾個項目(注意是用 index 計算)

```js
var removeItem = arr.splice(2, 2); // arr = ['Watermelon', 2];
```

## Function

Function 是 JavaScript 的一級物件(first class object)，組成依次為：

- 函式的名稱。
- 包圍在括號()中，並由逗號區隔的一個函式參數(argement)列表。
- 包圍在大括號{}中，內含需要重複執行的內容，是函式功能的主要區塊。

### Function 定義

1. 函式宣告（Function Declaration）
   宣告方式:

   - 宣告式（具名,匿名）

   ```js
   var add = function add(a,b); // 具名
   var add = function (a,b); // 匿名
   ```

2. 函式運算式（Function Expressions）
   宣告方式:

   - 表示式（具名,匿名）

   ```js
   function add(a,b); // 具名
   function (a,b); // 匿名
   ```

3. 透過 new Function 關鍵字建立函式
   直接使用 Function 這個關鍵字來建立函式物件。
   這裡要注意的是，如果使用這樣的方式建立函數，不會建立任何的 closure，而在這個函數中只能存取全域（global）的變數或是存在於該函數內部的變數。

   ```js
   var add = new Function("a", "b", "return a + b");

   add(1, 2);
   // 輸出 3

   var glbFoo = "global";
   function scope() {
     var scpFoo = "scoped",
       scop = Function("console.log(typeof scpFoo)"),
       glob = Function("console.log(typeof glbFoo)");

     scop(); // 只能讀到全域變數，函式本身沒有變數
     glob(); // glbFoo是全域變數，在執行scope()後是可以得到glbFoo的type
   }

   scope();
   // 輸出 undefined
   // 輸出 string
   ```

### 提升(Hoisting)

函式宣告(Function Declaration)與函式運算式(Function Expressions)的差異。
函式宣告建立的函數，會被提升（hoisting）到該作用域（scope）最頂端，讓整個作用域都可以呼叫它。

```js
declared();
// 輸出 declared
// 函式宣告function declaration
function declared() {
  console.log("declared");
}

expressed();
// TypeError: undefined is not a function
// 函式運算式function expressions
var expressed = function() {
  console.log("expressed");
};
expressed();
// 輸出 expressed
```

### IIFE

Immediately Invoked Functions Expressions 指的是可以立即執行的 Functions Expressions 函式表示式，中文多譯為立即(執行)函式。

下面是一個範例

```js
var hello = function(name) {
  console.log("Hello, " + name + "how are you?");
  // consolo.log會印出 Hello, undefined how are you?
};
```

這是一個 function expressions
呼叫他會寫成`hello();`。

```js
hello();
```

如果改成並把`hello();`拿掉

```js
var hello = (function(name) {
  console.log("Hello, " + name + "how are you?");
  // consolo.log會印出 Hello, undefined how are you?
})();
```

結果會是一樣的，因為在讀到函式運算是後面的`()`，就會立刻呼叫這個 function 去執行，這就是 IIFE。



### Function 參考資料

- [MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Functions)
- https://ithelp.ithome.com.tw/articles/10191549
- https://blog.gtwang.org/programming/defining-javascript-functions/


## 作用域(Scope)

全域變數(global)，在瀏覽器下也就是 window 物件，function 外的變數
區域變數，function 中的變數

傳統的後端程式語言，區域變數只在大括號{ }中有效，在此範圍外，就是全域變數，它是以區塊(block){ }作為區分，但 JavaScript 卻不是以此做區分，它是用函式(Function)作為區分，在函式內為區域變數，函式外為全域變數，要達成這樣的條件還有一個前提，變數必須以 var 宣告。

- 例子 1：雖然都是 scope，function 回傳的是區域變數的值，但最後的敘述式(statement)的值是全域變數的值。

  ```js
  var scope = "全域";
  function getValue() {
    var scope = "區域";
    return scope;
  }
  console.log(getValue()); //區域
  console.log(scope); //全域
  ```

- 例子 2：兩個 function 都有 scope，但不同區域的同命變數實際上是不同的。
  即使 scope 被宣告 3 次，但彼此之間沒有關係，都是獨立的變數。

  ```js
  var scope = "全域";
  function getValue1() {
    var scope = "區域1";
    return scope;
  }
  function getValue2() {
    var scope = "區域2";
    return scope;
  }
  console.log(getValue1()); //區域1
  console.log(getValue2()); //區域2
  console.log(scope); //全域
  ```

- 例子 3：如果不用 var 宣告
  結果會顯示同一個變數“區域”。
  不使用 var 這種作法很容易污染到全域(後來的 ES6 有 let 來改善 var 有時候會污染全域的問題)

  ```js
  scope = "全域";
  function getValue() {
    scope = "區域";
    return scope;
  }
  console.log(getValue()); //區域
  console.log(scope); //區域
  ```

- [參考來源](https://ithelp.ithome.com.tw/articles/10206604?sc=iThelpR)
