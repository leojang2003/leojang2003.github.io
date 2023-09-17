---
layout: post
title: Calculus Unit1-4 計算導數
subtitle: 
tags: ['Calculus 1A','18.01','OCW','Unit 1:Derivatives']
comments: true
---

### 課程目標

- {: .arrow} 找到簡單函數的**導數公式**。

- {: .arrow} 知道何時應用 **power rule**。

- {: .arrow} 將**線性**應用於微分線性組合。

在上一個單元中，我們學習了如何根據函數 f 的圖形繪製 f' 的圖形。現在我們想要找到 f' 的公式。我們將以計算 <img src="{{ 'assets/img/unit1/3/3-1.png' | relative_url }}" alt="" />的導數為例。在進行計算之前，讓我們試著預測 f' 的樣子。
<br/>

### 一個點的導數

我們知道 <img src="{{ 'assets/img/unit1/3/3-1.png' | relative_url }}" alt="" /> 的圖形如下<br class="new">

<img src="{{ 'assets/img/unit1/3/3-2.png' | relative_url }}" alt="" /><br class="new">

我們可以推斷曲線在 x > 0 時應為正值，在 x < 0 時應為負值，並且在 x = 0 處應等於 0。<br class="new">

假設我們要算 f'(3)，其算法如下<br class="new">

<img src="{{ 'assets/img/unit1/3/3-3.png' | relative_url }}" alt="" /><br class="new">

假設我們要算 f'(x)，其算法如下<br class="new">

<img src="{{ 'assets/img/unit1/3/3-4.png' | relative_url }}" alt="" /><br class="new">

我們可以知道 <img src="{{ 'assets/img/unit1/3/3-1.png' | relative_url }}" alt="" /> 的 f'(x) = 2x
<br/>

### 線性函數的導數

我們知道 <img src="{{ 'assets/img/unit1/3/4-1.png' | relative_url }}" alt="" /> 的圖形如下<br class="new">

<img src="{{ 'assets/img/unit1/3/4-2.png' | relative_url }}" alt="" /><br class="new">

我們知道 g'(x) 的圖形是一條非 0 的橫線。<br class="new">

<img src="{{ 'assets/img/unit1/3/4-3.png' | relative_url }}" alt="" /><br class="new">
<img src="{{ 'assets/img/unit1/3/4-4.png' | relative_url }}" alt="" /><br class="new">
<img src="{{ 'assets/img/unit1/3/4-5.png' | relative_url }}" alt="" /><br class="new">
<img src="{{ 'assets/img/unit1/3/4-6.png' | relative_url }}" alt="" /><br class="new">
<img src="{{ 'assets/img/unit1/3/4-7.png' | relative_url }}" alt="" /><br class="new">
<img src="{{ 'assets/img/unit1/3/4-8.png' | relative_url }}" alt="" /><br class="new">

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
因此，在點 x = a，函數 g 的切線斜率是函數 f 的切線斜率的2倍。換句話說，函數 g 的切線比函數 f 的切線更陡峭。
<br/>

### 常數倍數的導數 Derivatives of constant multiples

如果 <img src="{{ 'assets/img/unit1/3/7-2.png' | relative_url }}" alt="" />，f'(x) = ?<br class="new">
<solution>
我們可以將函數看成<img src="{{ 'assets/img/unit1/3/7-3.png' | relative_url }}" alt="" />，因此<br class="new">

<img src="{{ 'assets/img/unit1/3/7-4.png' | relative_url }}" alt="" />
<br/>

### 證明常數倍數的導數

假設對於所有的 x，有 g(x) = kf(x)，其中 k 是一個常數。我們想要證明在任何 f 可微的點 x 上，g′(x) = kf′(x)。<br class="new">

我們知道<br class="new">

<img src="{{ 'assets/img/unit1/3/8-1.png' | relative_url }}" alt="" />

第一個極限只是 k，而第二個極限是 f'(x) 的定義。因此，我們得到 g'(x) = kf'(x)。
<br/>

### 練習

假設 <img src="{{ 'assets/img/unit1/3/9-1.png' | relative_url }}" alt="" />，找出 <img src="{{ 'assets/img/unit1/3/9-2.png' | relative_url }}" alt="" /><br class="new">

這是兩個函數的差，因此它的導數是兩個導數的差。因此，<br class="new">

<img src="{{ 'assets/img/unit1/3/9-1.png' | relative_url }}" alt="" /><br class="new">

### 函數和的導數 = 導數的和

{: .note}
derivative of the sum is the sum of derivative

證明用 word 寫

### 導數與線性

如果對於某個常數 k，有 g(x) = k f(x)，那麼在所有 f 可微的點上，<br class="new">

g′(x) = k f′(x)。<br class="new">

如果 h(x) = f(x) + g(x)，那麼在所有 f 和 g 可微的點上，<br class="new">

h′(x) = f′(x) + g′(x)。<br class="new">

同樣的，如果 j(x) = f(x) - g(x)，那麼在所有 f 和 g 可微的點上，<br class="new">

j′(x) = f′(x) - g′(x)。<br class="new">
<br/>

我們已經看到微分有'重視'加法和常數的乘法。也就是說，如果對函數之和求導數，得到的結果與對每個部分分別求導數然後相加的結果相同。同樣的，如果對一個常數 k 乘以一個函數求導數，得到的結果就是原函數的導數乘以 k。<br class="new">

以這種方式遵守加法和常數乘法的特性被稱為“線性”，它是導數運算的一個重要屬性! 在下面的問題中，我們將結合線性的兩個方面。<br class="new">
<br/>

**南瓜速度**

南瓜的高度是 <img src="{{ 'assets/img/unit1/0/9-3.png' | relative_url }}" alt="" /> 公尺，表示從建築物上拋出的南瓜在 t 秒後距離地面的高度。我們可以計算在任意時間 t 的速度f'(t)。現在我們來計算 f'(t) 的值。<br class="new">

我們可以利用線性性質分別對每個項進行求導數。100 + 20t 的導數是 20，而 -5t² 的導數是 -5⋅(2t)，即-10t。綜上所述，我們得到 f'(t) = 20 - 5t²<br class="new">
ㄆㄇ
<br/>

**價格資增加的速率**

兒!<br class="new">

假設 2014 年披薩的價格是 12 美元，披薩價格的變化率為每年 0.2 美元。<br class="new">

與此同時，2014 年可口可樂的價格是 2 美元，其價格每年上漲 0.1 美元。<br class="new">

由於披薩和可樂的價格隨時間變化，派對的成本也隨之變化。令h(t)表示在年份 t 時 10 個披薩和 5 瓶可口可樂的總成本。即，h(t) = 10f(t) + 5g(t)，其中 f(t) 表示年份 t 時披薩的價格，g(t) 表示年份t時可樂的價格。<br class="new">
F
披薩派對的成本增長多快? 換句話說，h'(2014) 是多少?<br class="new">
<br/>

根據線性性質，我們知道 h′(t) = 10f′(t) + 5g′(t) 。因此，h′(2014)= 10*0.2 + 5*0.1 = 2.5 美元每年。

<br/>

**一個點的導數**

假設 <img src="{{ 'assets/img/unit1/3/12-1.png' | relative_url }}" alt="" />，我們想要計算 f'(1)。<br class="new">

首先，讓我們思考如何解決這樣的問題。一種方法是使用我們已經學過的求和規則和常數倍規則計算 f'(x)，然後將 x = 1 代入計算 f'(1)。另一種方法是先計算 f(1)，然後對結果進行求導。哪種方法是正確的呢？<br class="new">

A: 一般來說，我們在代入之前應該先求導數。代入一個值表示我們將注意力限制在一個點上，如果這樣做了，我們將失去函數變化的所有資訊(比如導數)。<br class="new">
<br/>

### Power Rule

用 word 打出證明

** 證明 a^n - b^n

用 word 打出證明

### Extended Power Rule

如果 n 是任何固定的數字(fixed number) 且 <img src="{{ 'assets/img/unit1/3/15-1.png' | relative_url }}" alt="" />，那麼 <img src="{{ 'assets/img/unit1/3/15-2.png' | relative_url }}" alt="" /><br class="new">

只是用 base 是 x，且指數必須要是固定的<br class="new">
<br/>

### 練習

我們知道 <img src="{{ 'assets/img/unit1/3/16-1.png' | relative_url }}" alt="" /> 以及<img src="{{ 'assets/img/unit1/3/16-2.png' | relative_url }}" alt="" />，所以這兩個式子與 <img src="{{ 'assets/img/unit1/3/16-3.png' | relative_url }}" alt="" />完全符合 power rule 的範本：輸入變數被提升到一個固定的指數。

函數 <img src="{{ 'assets/img/unit1/3/16-4.png' | relative_url }}" alt="" /> 和 <img src="{{ 'assets/img/unit1/3/16-5.png' | relative_url }}" alt="" />並沒有將輸入變數作為指數的底數，所以 power rule 不直接適用於它們。類似地，<img src="{{ 'assets/img/unit1/3/16-6.png' | relative_url }}" alt="" /> 中指數的底數是運算式 <img src="{{ 'assets/img/unit1/3/16-7.png' | relative_url }}" alt="" />，而不等於輸入變數 x。對於 <img src="{{ 'assets/img/unit1/3/16-7.png' | relative_url }}" alt="" />，指數不是一個固定的數字，因此 power rule 同樣不適用。
