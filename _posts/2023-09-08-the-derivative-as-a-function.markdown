---
layout: post
title: Calculus Unit1-3 導數作為一個函數
subtitle: 
tags: ['Calculus 1A','18.01','OCW','Unit 1:Derivatives']
comments: true
---

### 課程目標

- {: .arrow} 理解【在點上的導數】和【導數作為函數】之間的區別。

- {: .arrow} 將函數 f 的圖形與導數的正值和負值的區間進行相關聯。

- {: .arrow} 能夠根據函數 f 的圖形描繪導數 f′ 的圖形。

在微積分中，我們通常將函數和其圖形視為相同的對象。因此，為了研究函數，我們經常研究它們的圖形。<br class="new">

在上一部分中，我們了解在某些點上的導數是通過該點上的切線的斜率。當我們的圖形是平滑的，沒有不連續點、轉角或其他奇怪的行為時，我們可以找到任意點上切線的斜率。
<br/>

### 在 0 的導數

這是一個典型的函數 f 的圖形。讓我們開始思考導數 f' 的圖形可能想像的樣子。<br class="new">

<img src="{{ 'assets/img/unit1/2/4-1.png' | relative_url }}" alt="" /><br class="new">

根據圖形，我們可以在 0 畫一條切線，斜率估計為 3，f'(0) 的值是 3。<br class="new">

<img src="{{ 'assets/img/unit1/2/4-2.png' | relative_url }}" alt="" /><br class="new">

如果我們預測 f'(0) = 3，那表示著我們應該在 f' 的圖上標出點 (0,3) 如下。<br class="new">

<img src="{{ 'assets/img/unit1/2/5-1.png' | relative_url }}" alt="" /><br class="new">

f' 圖上的這一點，代表了點在 f 上對應的切線的斜率。我們可以推測 f'(1) 的值會小於 f'(0) 但還是正數。

<img src="{{ 'assets/img/unit1/2/5-2.png' | relative_url }}" alt="" /><br class="new">
<br/>

在 x = 1 ，f 圖表的切線不像在 x = 0 那樣斜。因此，f'(1) 小於 f'(0)。我們的 f' 圖表如下所示，向下傾斜。<br class="new">

<img src="{{ 'assets/img/unit1/2/6-1.png' | relative_url }}" alt="" /><br class="new">

我們從上面的圖形，推測 x 在 3 時 f'(x) = 0。<br class="new">

<img src="{{ 'assets/img/unit1/2/6-2.png' | relative_url }}" alt="" /><br class="new">

我們知道 f'(3) = 0 是因為 x = 3 是水平切線。因此，f' 在逐漸變小，直到在 x = 3 時變為零。<br class="new">

<img src="{{ 'assets/img/unit1/2/6-3.png' | relative_url }}" alt="" /><br class="new">

我們看到當 x 經過 x = 3 時，f' 變為負數，f 圖表的切線具有負斜率。因此，f' 的圖表應該如下所示<br class="new">

<img src="{{ 'assets/img/unit1/2/6-4.png' | relative_url }}" alt="" /><br class="new">

當 x = 5 時，切線的最陡負斜率出現。<br class="new">

<img src="{{ 'assets/img/unit1/2/9-1.png' | relative_url }}" alt="" /><br class="new">

f' 的圖在該點達到最負值。<br class="new">

<img src="{{ 'assets/img/unit1/2/9-2.png' | relative_url }}" alt="" /><br class="new">

當 x = 6 時，斜率為 0，x > 6 之後的斜率又為正。<br class="new">

<img src="{{ 'assets/img/unit1/2/10-1.png' | relative_url }}" alt="" /><br class="new">

### 第二個例子

<img src="{{ 'assets/img/unit1/2/11-1.png' | relative_url }}" alt="" /><br class="new">

我們觀察到一些現象，g'(3) = -2 ，在 x = -3 和 x = -1 之間的每一個點，切線的斜率都是 2。因此，在這個區間內，g′(x) 的值恆為 2。另外，我們可以看到在 x = -1 處，函數 g 的圖有一個 cornor，因此在該點不存在切線，即 g′(−1) 未定義。(雖然存在左導數為正和右導數為負的情況，但整體上導數不存在)。
<br class="new">

當 x < -1 時，g′(x) = 2，因此 g′ 的圖是一條水平線。我們使用一個開放的圓圈來表示由於 g 的圖中的 cornor，導致 g′(−1) 不存在的事實。點 (−1,2) 不在 g′ 的圖上。<br class="new">

<<第二個圖>>
<img src="{{ 'assets/img/unit1/2/12-1.png' | relative_url }}" alt="" /><br class="new">

在 -1 < x < 1 的區間內，函數 g 的圖上的所有切線斜率都為負。起初(接近 x = -1)，它們很陡，所以相對於更靠右(接近 x = 1)的地方，斜率較大且為負。當 x 從左側逼近 1 時，切線逐漸接近水平，因此 g′(x) 應該趨近於零。<br class="new">

<img src="{{ 'assets/img/unit1/2/12-2.png' | relative_url }}" alt="" /><br class="new">

其他觀察，在 x = 1 ，函數 g 的圖上存在一條切線，且該切線是水平的。因此，g′(1) = 0。<br class="new">

在 x > 1 的任意一點，函數 g 的圖上的切線都是水平的，因此在任何這樣的點上，導數應該為零。<br class="new">

<br/>
<br/>
<br/>
