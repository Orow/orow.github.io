---
title: "[JavaScript] - Recursion(遞迴)"
date: 2019-03-07 20:00:00
tags:
  - javascript
categories:
  - javascript
---

## Recursion(遞迴)

簡單的說，recursion 就是在一個函式當中再去呼叫它自己，其中一個實際的範例就是階層的計算（factorial）。例如 5! = 5 x 4 x 3 x 2 x 1。

### [範例參考連結](https://pjchender.blogspot.com/2017/09/recursive-function-recursion.html?m=1)

```js
function factorial(num) {
if (num === 1) {
    return num;
} else {
    return num * factorial(num - 1);
}
}
factorail(5); // 120
```

當 num 不等於 1 的時候，它會去呼叫自己（return num \* factorial(num-1)，如果 num 等於 1 時，才會回傳結果。
因此實際上執行的過程會像這樣：

<div><img src="https://i.imgur.com/U92Q9ms.gif"></div>

### Eloqument範例

[Eloqument](http://eloquentjavascript.net/03_functions.html)上也有一個例子可以做。
這本網路上的電子書可以直接在上面進行codinga練習並看到結果。
這個案例就像是2的三次方的計算方式

```js
function power(base, exponent) {
if (exponent == 0) {
    return 1;
} else {
    return base * power(base, exponent - 1);
}
}

console.log(power(2, 3));
// → 8
```

console.log(power(2,3))會變成以下方式
1. power(2,3) = return 2 x power(2, 2)
2. power(2,2) = return 2 x power(2, 1)
3. power(2,1) = return 2 x power(2, 0)
4. power(2,0) = return 1
再把得到的值往回推
5. power(2,0) = 1
6. power(2,1) = 2 x power(2, 0) = 2 x 1 = 2
7. power(2,2) = 2 x power(2, 1) = 2 x 2 = 4
8. power(2,3) = 2 x power(2, 2) = 2 x 4 = 8

