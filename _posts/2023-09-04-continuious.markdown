---
layout: post
title: Calculus Unit0-2 - 連續
subtitle: 
tags: ['Calculus 1A','18.01','OCW','Unit 0:Limits','Continuity','continuous','right-continuous','left-continuous','jump discontinuity','removable discontinuity','Intermediate Value Theorem','IVT']
comments: true
---
<br/>
### 連續性 Continuity

我們說一個函數 f 在點 x = a 處是連續的，如果<br class="new">

<img src="{{ 'assets/img/continuous/continuous-1-1.png' | relative_url }}" alt="" />

尤其是，如果 f(a) 或 <img src="{{ 'assets/img/continuous/continuous-1-2.png' | relative_url }}" alt="" /> 不存在，那麼 f 在 a 是不連續的。<br class="new">

我們說一個函數 f 在點 x = a 是右連續(right-continuous)的，如果 <img src="{{ 'assets/img/continuous/continuous-1-3.png' | relative_url }}" alt="" /><br/>

我們說一個函數 f 在點 x = a 是左連續(left-continuous)的，如果 <img src="{{ 'assets/img/continuous/continuous-1-4.png' | relative_url }}" alt="" /><br/>

### 跳躍不連續 Jump Discontinuity

有時候，知道不連續是哪種類型是很有用的。<br class="new">

如果左極限 <img src="{{ 'assets/img/continuous/continuous-1-5.png' | relative_url }}" alt="" /> 和右極限 <img src="{{ 'assets/img/continuous/continuous-1-6.png' | relative_url }}" alt="" /> 在點 x = a 都存在，但它們不相等，那麼我們說 f 在 x = a 有跳躍不連續(jump discontinuity)

<img src="{{ 'assets/img/continuous/continuous-1-7.png' | relative_url }}" alt="" />

### 可移除不連續性 Removable Discontinuity

如果整體極限(overall limit) <img src="{{ 'assets/img/continuous/continuous-1-8.png' | relative_url }}" alt="" /> 存在(即左右極限相等)，但整體極限不等於 f(a) ，那麼我們說 f 在 x = a 是可移除不連續性(removable discontinuity)

<img src="{{ 'assets/img/continuous/continuous-1-9.png' | relative_url }}" alt="" />

**問題1**

<img src="{{ 'assets/img/continuous/continuous-4.png' | relative_url }}" alt="" />

在 x = -2，f 既不是右連續也不是左連續。<br class="new">
在 x = -1，f 是連續的。<br class="new">
在 x = 1，f 既不是右連續也不是左連續。<br class="new">
在 x = 3，f 是右連續但不是左連續。<br class="new">
在 x = 4，f 是連續的。<br class="new">

**問題2**

<img src="{{ 'assets/img/continuous/continuous-5.png' | relative_url }}" alt="" />

在 x = -2, 1，是可移除不連續性。<br class="new">
在 x = 3，是跳躍不連續。<br class="new">
<br/>
### 整體連續性 Overall Continuity

我們說一個函數 f(x) 是連續的，是說在 f(x) domain 中的每個點 c ，函數 f 在點 x = c 是連續的。

**問題3**

{: .box-note}
Q : 如果函數 f 在 x = 3 是不連續的，而函數 g 在 x = 3 也是不連續的，那麼函數 h(x) = f(x) + g(x) 在 x = 3 也必定是不連續的?

{: .box-note}
A : 答案是錯的，假設函數 f 在 x = 3 是跳躍不連續，函數 g 在 x = 3 也是跳躍不連續，但是當將兩個函數相加時，跳躍點會互相抵消，從而得到一個連續的和函數。例如，如果 f(x) = ⌊x⌋ 且 g(x) = −⌊x⌋，那麼這兩個函數在 x = 3 都是跳躍不連續，但它們的和函數是常數函數 0，h(x) = 0 在任何點都是連續的。

  
  
### 連續函數的分類

以下函數在所有實數上都連續：

- {: .arrow} 所有多項式

- {: .arrow} <img src="{{ 'assets/img/continuous/continuous-19-1.png' | relative_url }}" alt="" />

- {: .arrow} <img src="{{ 'assets/img/continuous/continuous-19-5.png' | relative_url }}" alt="" />

- {: .arrow} cosx 和 sinx

- {: .arrow} 指數函數 <img src="{{ 'assets/img/continuous/continuous-19-2.png' | relative_url }}" alt="" /> ，其中底數 a > 0


以下函數在指定的 x 值上連續(或右連續)：

- {: .arrow} <img src="{{ 'assets/img/continuous/continuous-19-3.png' | relative_url }}" alt="" /> ，對於所有 x ≥ 0

- {: .arrow} tanx ，在所有定義域內的 x 上連續

- {: .arrow} 對數函數 <img src="{{ 'assets/img/continuous/continuous-19-4.png' | relative_url }}" alt="" /> ，其中底數 a > 0，x > 0


### Intermediate Value Theorem (IVT)

如果函數 f 在閉區間 [a,b] 上是**連續**的，並且 M 位於 f(a) 和 f(b) 的值之間，那麼至少存在一個點 c，它在 a 和 b 之間，滿足 f(c) = M。

{: .box-note}
當一個函數 f 在閉區間 [a,b] 上，在點 x = a 是右連續，在點 x = b 是左連續，並且在 a 和 b 之間的所有點上連續，則我們稱函數 f 在閉區間 [a,b] 上連續(**continuous on a closed interval**)。

IVT 之所以影響深遠，是因為它從局部的資訊，得出一個全域的結論。一個點上的連續性是局部資訊，因為它只需要瞭解函數在該點附近的行為。但是，如果我們知道區間上的每個點都是**連續**的，那麼 IVT 告訴我們一些關於整體或全域行為的資訊，即它的圖形必須穿過某條線。

這與實數 (real number) 系統的性質密切相關。有理數(可以表示為整數的分數)也是連續的，但是 IVT 不適用有理數。只有當我們使用實數時，IVT 才成立。

**問題1**

Q : 如果 g(x) = 4x^(−3) − x − sin(πx)。IVT 是否表示著在 x = −1 和 x = 1 之間必然存在某個 c，使得 g(c) = 0？

A: 不正確。請注意，g(x)在區間 [−1,1] 上不連續，因為它在 x = 0 未定義。因此，IVT 在這種情況下不適用。

**問題2**

Q : 一個多項式 p(x) 有以下的值<br class="new">

p(−2) 	 = 	 6 <br class="new">
p(−1) 	 = 	 −2 <br class="new">
p(0) 	 = 	 −3 <br class="new">
p(1) 	 = 	 1 <br class="new">
p(2) 	 = 	 6 <br class="new">

我們也知道 p(x) > 6 當 x > 2 或 x < −2 時，我們最多可以推論出什麼?<br class="new">

A : 注意，任何多項式在所有點上都是連續的，因此 IVT 適用於任何區間。
我們知道 p(−2) 是正數，p(−1) 是負數，因此在 x = −2 和 x = −1 之間至少存在一個使得 p 為 0 的點。同樣的，在 x = 0 和 x = 1 之間至少存在一個使得 p 為 0 的點。總而言之，我們知道至少有 2 個根(root)。<br class="new">


<br/>
<br/>
<br/>
