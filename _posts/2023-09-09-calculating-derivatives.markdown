---
layout: post
title: Calculus Unit1-4 計算導數
subtitle: 
tags: ['Calculus 1A','18.01','OCW','Unit 1:Derivatives']
comments: true
---

### 課程目標

- {: .arrow} 找到簡單函數的導數公式。

- {: .arrow} 知道何時應用冪規則。

- {: .arrow} 將線性性應用於微分線性組合。

在上一個單元中，我們學習了如何根據函數 f 的圖形繪製 f' 的圖形。現在我們想要找到 f' 的公式。我們將以計算 <img src="{{ 'assets/img/unit1/3/3-1.png' | relative_url }}" alt="" />的導數為例。在進行計算之前，讓我們試著預測 f' 的樣子。
<br/>

### 一個點的導數

我們知道 <img src="{{ 'assets/img/unit1/3/3-2.png' | relative_url }}" alt="" /> 的圖形如下<br class="new">

<img src="{{ 'assets/img/unit1/3/3-3.png' | relative_url }}" alt="" /><br class="new">

我們可以推斷曲線在 x > 0 時應為正值，在 x < 0 時應為負值，並且在 x = 0 處應等於 0。<br class="new">

假設我們要算 f'(3)，其算法如下

<<<用 word 的 math>>>

### 線性函數的導數

我們知道 <img src="{{ 'assets/img/unit1/3/4-1.png' | relative_url }}" alt="" /> 的圖形如下<br class="new">

<img src="{{ 'assets/img/unit1/3/4-2.png' | relative_url }}" alt="" /><br class="new">

我們知道 g'(x) 的圖形是一條非 0 的橫線。<br class="new">

{: .note}
如果 f(x) = ax + b，那麼 f'(x) = a 

### 使用 Delta Notation 

TBD

### 導數的線性度 Linearity of the derivative

到目前為止，我們使用導數的極限定義計算了以下函數的導數。<br class="new">

<img src="{{ 'assets/img/unit1/3/6-1.png' | relative_url }}" alt="" /><br class="new">

現在我們將使用這些公式來找到許多相關函數的導數。一個好處是對於這些函數，我們不需要使用極限定義來計算導數。<br class="new">

**圖形之間的關聯**

讓我們來舉個例子。假設我們知道函數 f(x) 的導數，然後我們有一個新函數 g，其中對於所有輸入 x，g(x) = 2f(x)。
為了從函數 f 的圖形得到函數 g 的圖形，我們需要對 f 的圖形進行什麼操作呢?<br class="new">

答案: 對於相同的 x 值，函數 g 的輸出是函數 f 輸出的兩倍。因此，函數 g 的圖形在垂直方向上被拉長了2倍，但在水平方向上沒有改變。<br class="new">
<br/>

**圖形之間的關聯**

如果 g(x) = 2f(x)，則函數 g 的圖形是函數 f 的圖形在垂直方向上被拉伸了2倍。那麼在點 x = a，函數 g 的切線斜率與函數 f 的切線斜率之間存在什麼關係呢?<br class="new">

<img src="{{ 'assets/img/unit1/3/7-1.png' | relative_url }}" alt="" /><br class="new">

由於 g(x) = 2f(x)，我們可以推斷 g'(x) = 2f'(x)，其中 g'(x) 和 f'(x) 分別表示函數 g 和函數 f 在 x 點處的導數。
因此，在點 x = a，函數 g 的切線斜率是函數 f 的切線斜率的2倍。換句話說，函數 g 的切線比函數 f 的切線更陡峭。<br class="new">

### 常數倍數的導數 Derivatives of constant multiples

如果 <img src="{{ 'assets/img/unit1/3/7-2.png' | relative_url }}" alt="" />，f'(x) = ?<br class="new">

我們可以將函數看成<img src="{{ 'assets/img/unit1/3/7-3.png' | relative_url }}" alt="" />，因此<br class="new">

<img src="{{ 'assets/img/unit1/3/7-4.png' | relative_url }}" alt="" />

### 證明常數倍數的導數
