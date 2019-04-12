---
title: "[JavaScript] - 練習-Simple JavaScript Quiz"
date: 2019-03-14 14:18:43
tags:
  - javascript
category:
  - project
---

## Demo

JavaScript 練習-Simple JS Quiz- [Demo](https://orow.github.io/MyProjects/ProjectsInJS&jQuery/SimpleJavascriptQuiz/index.html)

## 1. Introduction 介紹

這是在Udemy上課程的一個測驗的表單，利用 radio 的方式來選擇答案，會判斷每一題都要選擇，否則會 alert 提示訊息。
比對選取的答案後會有分數計算後，塞回原本 form 表單的上面顯示得分數。
這邊form用了`onsubmit="return submitAnswers()"`，代表submit送出後會回傳執行`submitAnswer()`後的結果。
![](https://i.imgur.com/UybpcEn.png)

## 2. 功能

第一題中的選項全部的 name 屬性為`name="q1"`,第二題全部選項皆為`name="q2"`，以此類推，value 則是一樣用 a, b, c, d 來表示個別選項的 value。

html程式碼：

```html
<section>
  <div id="results"></div>
  <form name="quizForm" onsubmit="return submitAnswers()">
    <h3>1. In which HTML element do we put in JavaScript code?</h3>
    <input type="radio" name="q1" value="a" id="q1a">a. &lt;js&gt;<br>
    <input type="radio" name="q1" value="b" id="q1b">b. &lt;script&gt;<br>
    <input type="radio" name="q1" value="c" id="q1c">c. &lt;body&gt;<br>
    <input type="radio" name="q1" value="d" id="q1d">d. &lt;link&gt;<br>

    <h3>2. Which HTML attribute is used to reference an external JavaScript file?</h3>
    <input type="radio" name="q2" value="a" id="q2a">a. src<br>
    <input type="radio" name="q2" value="b" id="q2b">b. rel<br>
    <input type="radio" name="q2" value="c" id="q2c">c. type<br>
    <input type="radio" name="q2" value="d" id="q2d">d. href<br>

    <h3>3. How would you write "Hello" in an alert box?</h3>
    <input type="radio" name="q3" value="a" id="q3a">a. msg("Hello");<br>
    <input type="radio" name="q3" value="b" id="q3b">b. alertBox("Hello");<br>
    <input type="radio" name="q3" value="c" id="q3c">c. document.write("Hello");<br>
    <input type="radio" name="q3" value="d" id="q3d">d. alert("Hello");<br>

    <h3>4. JavaScript is directly related to the "Java" programming language</h3>
    <input type="radio" name="q4" value="a" id="q4a">a. True<br>
    <input type="radio" name="q4" value="b" id="q4b">b. False<br>

    <h3>5. A variable in JavaScript must start with which special character</h3>
    <input type="radio" name="q5" value="a" id="q5a">a. @<br>
    <input type="radio" name="q5" value="b" id="q5b">b. $<br>
    <input type="radio" name="q5" value="c" id="q5c">c. #<br>
    <input type="radio" name="q5" value="d" id="q5d">d. No Special Character<br>
    <br>
    <br>
    <input type="submit" value="Submit Answers">
  </form>
</section>
```

JS的部分：
先判定每一題都要選到，不能為空值。
把全部正確答案設定成陣列後，進行判斷並加上分數。
將分數的結果用innerHTML塞回html中。

關於分數顯示，先宣告總分跟初始分數，也先把每一題選擇後的value宣告好。

```js
var total = 5;
var score = 0;

// Get User Input
var q1 = document.forms["quizForm"]["q1"].value;
var q2 = document.forms["quizForm"]["q2"].value;
var q3 = document.forms["quizForm"]["q3"].value;
var q4 = document.forms["quizForm"]["q4"].value;
var q5 = document.forms["quizForm"]["q5"].value;
```

第一部分則是先判定每一題都需要選到，用for迴圈來判定每一題不是null或是空值，如果是的會就要alert提示，並用return false來停止之後的迴圈執行。

```js
for (var i = 1; i <= total; i++) {
    // qi, q固定i是迴圈=>eval('q'+ i )
    if (eval('q' + i) == null || eval('q' + i) == "") {
        alert('You missed question ' + i);
        return false;
    }
}
```

接著比對答案，如果正確則score需要加1。

```js
// Set Correct Answers
var answers = ["b", "a", "d", "b", "d"];

// Check Answers
for (var i = 1; i <= total; i++) {
    if (eval('q'+i) == answers[i-1]) {
        score++;
    }
}
```

最後把score加回html中。

```js
// Display Results
var results = document.getElementById('results');
results.innerHTML = '<h3>You scored <span>' +score+ '</span> out of <span>' +total+'</span></h3>';
alert('You scroed ' +score+ ' out of' +total);
return false;
```
