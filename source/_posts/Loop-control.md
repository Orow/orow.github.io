---
title: Control & Loop（控制判斷與迴圈）
date: 2019-03-06 20:00:00
tags:
  - javascript
categories:
  - javascript
---

## Control Flow

JavaScript 用來控制流程的「條件語法」指的是，當指定的條件為 true 時，就會執行後續所指定的指令。 而 JavaScript 的條件語法有兩種︰ if...else 和 switch。

### if..else

如同字面上，如果指定條件為`true`就執行某件事，否則`else`就做另一件事。

```js
if (指定條件) {
  陳述式 1;
} else {
  陳述式 2;
}
```

如果有多個不同的條件也可以藉由 else if 來使用複合的陳述式來測試，如下：

```js
if (指定條件1) {
  陳述式 1;
} else if (指定條件 2) {
  陳述式 2;
} else if (指定條件 n) {
  陳述式 n;
} else {
  最後陳述式;
}
```

- [參考來源](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Control_flow_and_error_handling)

### switch

JavaScript 遇到 switch 語句會先執行指定的 expression 語句，然後用執行 expression 得到的值，去跟所有 case 的值做比較，如果相等就執行這個 case 區塊的程式碼，都不相等則執行 default 區塊的程式碼。

```js
switch (expression) {
  case value_1:
    statements_1
    [break;]
  case value_2:
    statements_2
    [break;]
    ...
  default:
    statements_def
    [break;]
}
```

例子：

```js
switch (i) {
  case 1:
    alert("i = 1");
    break;
  case 2:
    alert("i = 2");
    break;
  case 3:
    alert("i = 3");
    break;
  default:
    alert("i > 3 or i <= 0");
    break;
}
```

如果變數 i 的值 1，則執行 case 1 區塊的程式碼，以此類推;如果都不在指定的 case 裡面，則會執行 default 區塊的程式碼。

關鍵字 break
當 JavaScript 執行到 break 這個關鍵字的時後，就會直接跳出整個 switch 區塊，繼續往下執行。
如果沒有`break`，程式會從符合的 case 區塊開始一路往下執行，直到遇到 break 為止。

相較來說 switch 得效能比較好一些

- if...else 會先判斷後再執行
- switch 一定會執行，但還是會先比對 case

* [參考來源 1](https://www.fooish.com/javascript/switch-case.html)
* [參考來源 2](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Control_flow_and_error_handling)

## Loop

迴圈(loop)指的是，想要重複做某件事情，數值則會依次數而有`遞增`或`遞減`的變化來完成結束的條件。
主要分為

- for
- while
- do..while

### for

直接看例子

```js
for (let i = 0; i < 5; i++) {
  console.log(i);
}
```

先定義初始值`let i=0`
然後`i < 5`是結束條件，意思就是如果 i 大於等於 5 就會結束。
`i++`為結束每一回合時變動，意思就是`i = i + 1`。

### while

while 基本上概念與 for 是相同的，看下面兩格結果是相同的例子

```js
// for
for (let i = 0; i < 5; i++) {
  console.log(i);
}

// while
let i = 0;
while (i < 5) {
  console.log(i);
  i += 1;
}
```

`while` 的做法是 while(條件){行為}，上面的例子 `i < 5` 是條件，每一次的行為是印出 `i` 後再讓 i+1 ，之後當 `i = 4` 這次行為結束後 i+1 變成 5 ，下一次開始判斷條件就不符合，迴圈即終止。

兩者相較

- while 是控制條件的變數在迴圈開始前即存在，迴圈開始後只會定義條件，不會去初始化參數，大多數用在迴圈執行次數『不確定』的時候。
- for 則在迴圈一開始就定義變數，大多數用在迴圈次數『明確』的狀態。

### do...while

do...while 則是跟 while 幾乎一樣，只是 do...while 可以確保迴圈的第一次行為被執行。
下面例子結果與上述 for、while 方式會多一次，因為到了 i=4 這次之後用 console.log 印出後，最後還有執行一次 while (i < 5);所以還是會再執行一次 do 的行為。

```js
// do...while
let i = 0;
do {
  console.log(i);
  i += 1;
} while (i < 5);
```

### 迴圈操作

break 與 continue 是迴圈中兩種特別的指令

- break
  會直接中斷跳離迴圈。
- continue
  會讓迴圈跳過這一次的行為繼續執行下一次的迴圈。

Break 例子：如果找到所要的東西，就停止迴圈

```js
while (true) {
  const result = foo();
  if (result == "find it") {
    console.log("找到了，結束迴圈");
    break;
  }
}
```

Continue 例子：假設想要印出 1 ~ 10的數字 ，但是不包括 3 的倍數。

```js
for (let i = 1; i <= 10; i++) {
  // i 能被 3 整除表示 i 是 3 的倍數，遇到 continue 就會跳過這次
  if( i % 3 === 0){
    continue;
  }
  console.log(i);
}
```

* [參考來源 1](https://tigercosmos.xyz/master-js-in-30-days/PART1/loop.html)
* [參考來源 2](https://ithelp.ithome.com.tw/articles/10191453)
