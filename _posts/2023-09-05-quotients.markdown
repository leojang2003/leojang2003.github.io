---
layout: post
title: Calculus Unit0-3 - Limit of Quotients
subtitle: 
tags: ['Calculus 1A','18.01','OCW','Unit 0:Limits','Limits of quotients','Limit Law for Division']
comments: true
---
<br/>
### Quotients of small numbers

讓我們考慮一個一般情況的極限<img src="{{ 'assets/img/quotients/quotients-1.png' | relative_url }}" alt="" />，其中分子和分母都趨近於 0。

當 x 接近 a 時，f(x) 和 g(x) 都接近於零，但關鍵是它們不一定等於零。因此，要評估 <img src="{{ 'assets/img/quotients/quotients-1.png' | relative_url }}" alt="" />，我們需要弄清楚當我們將一個小數字除以另一個小數字的時候會發生什麼。我們會要求你實際將一些非常小的數字除以另外一些非常小的數字，以便自己找出答案！

<br/>

### 極限除法定律

如果 <img src="{{ 'assets/img/quotients/6-1.png' | relative_url }}" alt="" /> 且 <img src="{{ 'assets/img/quotients/6-2.png' | relative_url }}" alt="" />，則：

如果 M ≠ 0，則 <img src="{{ 'assets/img/quotients/6-3.png' | relative_url }}" alt="" />。

如果 M = 0 但 L ≠ 0，則 <img src="{{ 'assets/img/quotients/6-4.png' | relative_url }}" alt="" /> 不存在。

如果 M = 0 且 L = 0，則 <img src="{{ 'assets/img/quotients/6-1.png' | relative_url }}" alt="" /> 可能存在，也可能不存在。需要更多的工作來確定最後一種類型的極限是否存在，以及如果存在的話是什麼。
<br/>

### 極限定律問題

假設 <img src="{{ 'assets/img/quotients/7-1.png' | relative_url }}" alt="" />  <br/>

<img src="{{ 'assets/img/quotients/7-2.png' | relative_url }}" alt="" />  = 0 <br class="new">

乘法的極限法則沒有例外，所以第一個極限將是 17 * 0 = 0。
<br/>

<img src="{{ 'assets/img/quotients/7-3.png' | relative_url }}" alt="" />  = 0 <br class="new">

分子趨近於 17，而分母趨近於 0。這表示著商的極限不存在。
<br/>

<img src="{{ 'assets/img/quotients/7-4.png' | relative_url }}" alt="" />  = 0 <br class="new">

同樣地，加法的極限法則告訴我們第三個極限將是 0 + 17 + 0 = 17。
<br/>

<img src="{{ 'assets/img/quotients/7-5.png' | relative_url }}" alt="" />  = 0 <br class="new">

分子和分母都趨近於 0。僅憑這個訊息我們無法得出任何結論；我們需要做更多的工作來找出極限是什麼。
<br/>

<img src="{{ 'assets/img/quotients/7-6.png' | relative_url }}" alt="" />  = 0 <br class="new">

分子趨近於 0 + 0，而分母趨近於 17。這表示著商的極限為 0 / 17 = 0。
<br/>

### 除法極限定律練習

<img src="{{ 'assets/img/quotients/9-1.png' | relative_url }}" alt="" />  <br class="new">

上述的 limit 不存在，因為分子趨近 3，分母趨近 0。
<br/>

<img src="{{ 'assets/img/quotients/9-2.png' | relative_url }}" alt="" />  <br class="new">

= (1+x^3)/(x^2-3x) = 9/-2 = -4.5
<br/>

<img src="{{ 'assets/img/quotients/9-3.png' | relative_url }}" alt="" />  <br class="new">

= ((12/x)-3x)/(2-3x+x^2) = (3x^2-12)/-x(x-1)(x-2) = (3x+6)(x-2)/-x(x-1)(x-2) = (3x+6)/-x(x-1) = -6
<br/>

<img src="{{ 'assets/img/quotients/9-4.png' | relative_url }}" alt="" />  <br class="new">

= 2(x-2)(x-3)/x(x-3)(x-3) = 2(x-2)/x(x-3) = 2/0 = DNE
<br/>

<img src="{{ 'assets/img/quotients/9-5.png' | relative_url }}" alt="" />  <br class="new">

= (3x^2+x)/x^2+x+1 = 0/1 = 0
<br/>

### 基本極限概念

通常來說，如果 f 是一個函數，那麼 <img src="{{ 'assets/img/quotients/10-1.png' | relative_url }}" alt="" />  <br class="new">

答案 : False <br class="new">
<br/>

如果 <img src="{{ 'assets/img/quotients/10-2.png' | relative_url }}" alt="" />  <br class="new">

答案 : True
<br/> 

計算 <img src="{{ 'assets/img/quotients/10-3.png' | relative_url }}" alt="" />  <br class="new">

答案 : 2
<br/>

計算 <img src="{{ 'assets/img/quotients/10-4.png' | relative_url }}" alt="" />  <br class="new">

答案 : 0
<br/>

計算 <img src="{{ 'assets/img/quotients/10-5.png' | relative_url }}" alt="" />  <br class="new">

答案 : 3
<br/>

### 無窮的極限

計算 <img src="{{ 'assets/img/quotients/11-1.png' | relative_url }}" alt="" />  <br class="new">

答案 : <img src="{{ 'assets/img/quotients/11-2.png' | relative_url }}" alt="" />  <br class="new">
<br/>

在我們知道 <img src="{{ 'assets/img/quotients/11-3.png' | relative_url }}" alt="" /> 與 <img src="{{ 'assets/img/quotients/11-4.png' | relative_url }}" alt="" /> 後，<img src="{{ 'assets/img/quotients/11-5.png' | relative_url }}" alt="" /> 的值是多少? <br class="new">

答案 : 極限不存在，不是 <img src="{{ 'assets/img/quotients/11-6.png' | relative_url }}" alt="" />  <br class="new"> 也不是 <img src="{{ 'assets/img/quotients/11-7.png' | relative_url }}" alt="" />
<br/>

### 另一個函數

計算 <img src="{{ 'assets/img/quotients/12-1.png' | relative_url }}" alt="" />  <br class="new">

答案 : <img src="{{ 'assets/img/quotients/12-2.png' | relative_url }}" alt="" />  <br class="new">
<br/>

計算 <img src="{{ 'assets/img/quotients/12-3.png' | relative_url }}" alt="" />  <br class="new">

答案 : <img src="{{ 'assets/img/quotients/12-2.png' | relative_url }}" alt="" />  <br class="new">
<br/>

在我們知道 <img src="{{ 'assets/img/quotients/12-1.png' | relative_url }}" alt="" /> 與 <img src="{{ 'assets/img/quotients/12-3.png' | relative_url }}" alt="" /> 後，<img src="{{ 'assets/img/quotients/12-5.png' | relative_url }}" alt="" /> 的值是多少? <br class="new">

答案 : <img src="{{ 'assets/img/quotients/12-2.png' | relative_url }}" alt="" />  <br class="new">
<br/>

### 牽涉到無窮極限的除法

如果 <img src="{{ 'assets/img/quotients/15-1.png' | relative_url }}" alt="" /> 且 <img src="{{ 'assets/img/quotients/15-2.png' | relative_url }}" alt="" /> 那麼 <img src="{{ 'assets/img/quotients/15-3.png' | relative_url }}" alt="" /> 是多少? <br class="new">

答案 : 0
<br/>

計算 <img src="{{ 'assets/img/quotients/15-4.png' | relative_url }}" alt="" />  <br class="new">

答案 : <img src="{{ 'assets/img/quotients/15-5.png' | relative_url }}" alt="" />  <br class="new">
<br/>

<br/>
<br/>
<br/>
