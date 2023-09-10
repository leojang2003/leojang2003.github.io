---
layout: post
title: Geometric interpretation of the derivative
subtitle: 
tags: ['Calculus 1A','18.01','OCW','Unit 1:Derivatives','tangent line','secant line','step function',cornor function','slope','right-side derivative','left-side derivative']
comments: true
---
<br/>

### 導數的解釋

導數可以有三種不同的解釋<br class="new">

1. 物理上的解釋 : 瞬間變化率<br class="new">

2. 幾何上的解釋 : 切線的斜率<br class="new">

3. 函數對小改變的敏感度(sensitivity of function to small changes)<br class="new">

我們現在要來探討第二種在幾何上導數的意義為何
<br/>

### 切線 Tangent lines

{:. box-note}
在幾何學中，曲線上的切線（簡稱切線）是在給定點處與曲線“恰好接觸”的直線

我們已經看到了我們可以畫一條切線。切線有一種特殊性質，即切線通過(go through)函數的某一點，但並不穿過(cut across)該點的函數。<br class="new">

讓我們花一點時間思考切線的本質。首先，讓我們從一個錯誤的理解方式開始討論切線。很多學生在高中學習時認為切線是“只在一個點上接觸曲線的直線”。對於圓形曲線來說這是正確的，但對於其他許多曲線和函數來說，這個定義是錯誤的。讓我們看看為什麼。<br class="new">

<img src="{{ 'assets/img/unit1/1/3-1.png' | relative_url }}" alt="" /><br class="new"> <br class="new">

上面這條線是切線嗎? 不是 <br class="new">

<img src="{{ 'assets/img/unit1/1/3-2.png' | relative_url }}" alt="" /><br class="new"> <br class="new">

上面這條線是在標記點是切線嗎? 是 <br class="new">
<br/>

以下圖形為 f(x) = sin(πx)，我們可以看到切線同時通過多個點，因此切線有可能同時是**多個點**的切線

<img src="{{ 'assets/img/unit1/1/4-1.png' | relative_url }}" alt="" />
<br/>

### 切線與微積分的關係

假設有一個函數 f(x)，有一個點 a，剛好有一條切線通過點 a，我們要怎麼知道這條切線是什麼?

<img src="{{ 'assets/img/unit1/1/2-1.png' | relative_url }}" alt="" /><br class="new">

因為這條切線通過 (a, f(a)) 根據斜率的算法，斜率是 rise/run = △y/△x，因此我們知道這條切線的公式為<br class="new">

y - f(a) = m × (x - a)<br class="new">

如果我們相要知道這個斜率 m 是多少，這時就要利用微積分來幫我們算出這個值了! 後續我們就會知道其實<br class="new">

m = f'(a)<br class="new">

<br/>

### 切線的直覺

使用課程提供的模擬器，我們可以看到 f(x) = 0.5 × x^3 - x 的圖形如下，我們可以看到橘色的線是切線，藍色的線是函數 <br class="new">

<img src="{{ 'assets/img/unit1/1/4-2.png' | relative_url }}" alt="" /> <br class="new">

當我們拉近這個點時，我們可以看到藍色的函數越來越接近直線<br class="new">

<img src="{{ 'assets/img/unit1/1/4-3.png' | relative_url }}" alt="" /> <br class="new">

持續拉近，我們可以看到藍色的函數越來越接近直線 <br class="new">

<img src="{{ 'assets/img/unit1/1/4-4.png' | relative_url }}" alt="" /> <br class="new">

藍色函數在放大一千倍時，已經趨近於直線了 <br class="new">

<img src="{{ 'assets/img/unit1/1/4-5.png' | relative_url }}" alt="" /> <br class="new">
<br/>

換另一個例子，函數為 x 取絕對值圖形如下，當 x 為負值時，切線的斜率固定是 -1，可以看到圖上的 f'(x) = -1<br class="new">

<img src="{{ 'assets/img/unit1/1/4-7.png' | relative_url }}" alt="" /> <br class="new">

當 x 為正值時，切線的斜率固定是 1，可以看到圖上的 f'(x) = 1<br class="new">

<img src="{{ 'assets/img/unit1/1/4-8.png' | relative_url }}" alt="" /> <br class="new">

但是當 x 為 0 時，是沒有切線的，因為就算我們無限放大這個函數如下圖，不管我們怎麼放大，函數還是長一樣，沒有趨近成一條直線<br class="new">

<img src="{{ 'assets/img/unit1/1/4-9.png' | relative_url }}" alt="" /> <br class="new">

因此函數在 x = 0 的 f'(0) 不存在<br class="new">

<img src="{{ 'assets/img/unit1/1/4-6.png' | relative_url }}" alt="" /> <br class="new">

<br/>

### 割線的斜率

為了要計算切線的斜率，我們引入了割線的觀念，我們想要計算割線的斜率，斜率的定義為 **rise**/**run** = △y/△x<br class="new">

<img src="{{ 'assets/img/unit1/1/5-1.png' | relative_url }}" alt="" /> <br class="new">
<br/>

### 平均變化率

我們想要引入函數 f 的圖形，並將其與導數和變化率相連接。讓我們回顧一下我們之前的內容：<br class="new">

函數 f 在 x = a 和 x = b 之間的平均變化率是? <br class="new">

<img src="{{ 'assets/img/unit1/0/9-7.png' | relative_url }}" alt="" />
<br/>

### 幾何上的平均變化率

在函數 f 的圖形上，x = a 和 x = b 對應於點 (a, f(a)) 和 (b, f(b))。平均變化率 <img src="{{ 'assets/img/unit1/1/8-0.png' | relative_url }}" alt="" /> 在幾何上的意義是?<br class="new">

<img src="{{ 'assets/img/unit1/1/6-1.png' | relative_url }}" alt="" /><br class="new">

平均變化率 = 割線(Secant Line)的斜率。
<br/>

### 割線/切線 

我們的目標是找到切線的斜率，但我們不知道怎麼做，所以引入割線的概念。<br class="new">

<img src="{{ 'assets/img/unit1/1/7-1.png' | relative_url }}" alt="" /><br class="new">

從上述的圖可以看到，割線的斜率 = 相對於 x 的函數 f(x) 的平均變化率<br class="new">

<br/>
### 割線斜率的極限

現在，當我們定義函數 f 在 a 處的導數 f'(a) 時，當 b 接近 a 時我們取 <img src="{{ 'assets/img/unit1/1/8-0.png' | relative_url }}" alt="" /> 的極限，這個值也是連接點 (a,f(a)) 和 (b,f(b)) 的割線的斜率。<br class="new">

我們現在要的問題是，當 b 接近 a 時，割線的極限是多少?  我們可以從以下模擬器可以略知一二

<br/>

我們可以從下圖得知，當 b 無限接近 a 時，割線就趨近於切線，割線的極限就是切線的斜率。<br class="new">

<img src="{{ 'assets/img/unit1/1/8-1.png' | relative_url }}" alt="" /><br class="new">

△x 越來越小，割線往切線靠近<br class="new">
<img src="{{ 'assets/img/unit1/1/8-2.png' | relative_url }}" alt="" /><br class="new">

當 △x 趨近於 0 時，割線近乎與切線重疊<br class="new">

<img src="{{ 'assets/img/unit1/1/8-3.png' | relative_url }}" alt="" /><br class="new">
<br/>

### 割線的極限

<img src="{{ 'assets/img/unit1/1/9-1.png' | relative_url }}" alt="" /><br class="new">

<img src="{{ 'assets/img/unit1/1/9-2.png' | relative_url }}" alt="" /><br class="new">
<br/>

### 推測導數 Estimate derivatives

以下是函數 g 的圖形<br class="new">

<img src="{{ 'assets/img/unit1/1/12-1.png' | relative_url }}" alt="" /><br class="new">

Q: 切線的斜率在 x = 1 的值是正值、負值還是 0 ?<br class="new">

A: 負值<br class="new">

找出以下點的推測導數為何?<br class="new">

<img src="{{ 'assets/img/unit1/1/12-2.png' | relative_url }}" alt="" /><br class="new">

g'(-1) ≒ 2<br class="new">
g'(0)  ≒ 0<br class="new">
g'(1)  ≒ -1<br class="new">
g'(2)  ≒ 0<br class="new">

### 線性函數

如果圖是一條線 f，斜率是 1/2 如下<br class="new">

<img src="{{ 'assets/img/unit1/1/12-1.png' | relative_url }}" alt="" /><br class="new">

Q: 如果我們取兩個點，割線的斜率是?<br class="new">

A: 0.5

Q: f'(3) = ?

A: 0.5

### 當切線不存在時...導數不存在

切線的斜率，也被稱為導數，只有當切線存在時才存在！讓我們探討一些切線不存在(因此導數也不存在)的情況。<br class="new">

{: .note}
No tangent line => No derivative


**絕對值**

<img src="{{ 'assets/img/unit1/1/14-1.png' | relative_url }}" alt="" />

Q: 這是絕對值函數<img src="{{ 'assets/img/unit1/1/14-2.png' | relative_url }}" alt="" />的圖像。當你向原點拉近時，圖像會越來越像一條直線嗎?<br class="new">

A: 不，因此在原點沒有切線(因此也無導數)。<br class="new">

我們知道 <img src="{{ 'assets/img/unit1/1/14-2.png' | relative_url }}" alt="" /> 在 x = 0 時是沒有切線的。這種函數叫做 cornor function。<br class="new">
<br/>

**左右極限**

再來我們計算絕對值函數的左右極限<br class="new">

<img src="{{ 'assets/img/unit1/1/14-3.png' | relative_url }}" alt="" /><br class="new">

算出原點處左右割線斜率的極限<br class="new">

<img src="{{ 'assets/img/unit1/1/14-4.png' | relative_url }}" alt="" /> = 1<br class="new">

<img src="{{ 'assets/img/unit1/1/14-5.png' | relative_url }}" alt="" /> = -1<br class="new">

在原點的左右極限不相等，因此原點的極限不存在，原點不存在導數。但如果我們只看右側的話，切線與割線是同一條線，斜率為 1，因此我們稱 f 在 x = 0 的右導數 (right-side derivative) f'(0+) = 1。同樣可以推測， f 在 x = 0 的左導數 (left-side derivative) f'(0-) = -1。左右導數不相等，因此 f 在 x = 0 無導數。

<img src="{{ 'assets/img/unit1/1/15-1.png' | relative_url }}" alt="" /> = -1<br class="new">

<br/>

**跳躍不連續**

再來看一個例子，以下是 step 函數<br class="new">

<img src="{{ 'assets/img/unit1/1/14-6.png' | relative_url }}" alt="" /> = -1<br class="new">

如果我們在點 (0,1) 處放大，我們會看到一個 Ray。從左側趨近於 0，切線在 x = 0 的時候垂直，因此 f'(0-) = <img src="{{ 'assets/img/unit1/1/15-1.png' | relative_url }}" alt="" />，我們可以說不存在左導數。從右側趨近 0 的切線就是斜率 0，右導數為 0。左右導數不相同，因此 f 在 x = 0 無導數。

x 從左側趨近於 0 時，我們可以看到切線逐漸變成垂直切線<br class="new">

<img src="{{ 'assets/img/unit1/1/15-2.png' | relative_url }}" alt="" /> = -1<br class="new">

x = 0 變成垂直切線<br class="new">

<img src="{{ 'assets/img/unit1/1/15-2.png' | relative_url }}" alt="" /> = -1<br class="new">

{:. note}
垂直切線的斜率不存在

### 連續與可微分的關係

從上述的圖形我們可以知道

- 如果 f 在 x = a 不是連續的，那麼 f 在 x = a 是不可微分的。
- 如果 f 在 x = a 是連續的，那麼 f 在 x = a 不一定是可微分的。(垂直切線)

### 當切線存在時...導數有可能不存在

考慮 <img src="{{ 'assets/img/unit1/1/19-2.png' | relative_url }}" alt="" /> ，x = 0 的切線為垂直的，因此不存在導數。

### 找出以下的 f'(x)

<img src="{{ 'assets/img/unit1/1/16-1.png' | relative_url }}" alt="" /><br class="new">

f'(-1) = DNE<br class="new">
f'(0) = 0<br class="new">
f'(1) = DNE<br class="new">
f'(2) = DNE<br class="new">
f'(3) = 0.2<br class="new">

### 切線的方程式

Q: 找出函數 <img src="{{ 'assets/img/unit1/1/17-1.png' | relative_url }}" alt="" /> 在 x = 1 的切線，我們知道切線會經過的點是? <br class="new">

A: (1,5)<br class="new">

使用導數的定義來計算 <img src="{{ 'assets/img/unit1/1/17-2.png' | relative_url }}" alt="" /> 的圖在點 x = 1 處的切線斜率。<br class="new">

A: 7<br class="new">

有了點與斜率，我們可以推出算出 tangent 的函數為 y = mx + b = 7x - 2

### 練習

<img src="{{ 'assets/img/unit1/1/18-1.png' | relative_url }}" alt="" />

Q: 這是一個水塔在夏季期間水量的圖表。請估算第10天和第40天時水量隨時間的瞬間變化率。不要忘記單位!<br class="new">

A: 要推算第 10 天的瞬間變化率，使用公式取 (f(10)-f(x))/10-x 極限當 x -> 10，因為這裡是用天數，所以最趨近於第 10 天就是 x = 9，根據圖表 f(10) ≒ 550,000，f(9) ≒ 540,000，所以我們可以推估 f'(10) = (550000-540000)/(10-9) = 10000，同理 f'(40) = -10000。<br class="new">

Q: 假設圖形 f 在 x = 3 有切線，切線的方程式是 y = -2x + 1，那麼 f(3) 與 f'(3) 分別是什麼?<br class="new">

A: f(3) = -5, f'(3) = -2<br class="new">

### 總結

**割線**

函數 f(x) 在區間 a ≤ x ≤ b 上的割線是通過點 (a,f(a)) 和 (b,f(b)) 的直線。<br class="new">

割線的斜率是 <img src="{{ 'assets/img/unit1/1/8-0.png' | relative_url }}" alt="" />，它表示函數 f(x) 在區間 a ≤ x ≤ b 上的平均變化率。<br class="new">

割線的方程為 <img src="{{ 'assets/img/unit1/1/19-0.png' | relative_url }}" alt="" />。<br class="new">
<br/>

**切線**

函數 f(x) 在 x = a 的切線是通過點 (a,f(a))的直線，且該線的斜率為 f(x) 在 x = a 的瞬間變化率。這個斜率是你將函數放大到看起來像一條直線時得到的直線的斜率。<br class="new">

切線的斜率為 f′(a)。<br class="new">

切線的方程式為 <img src="{{ 'assets/img/unit1/1/19-1.png' | relative_url }}" alt="" />。<br class="new">
<br/>

**切線的屬性**

如果函數 f(x) 在 x = a 存在導數，那麼切線也存在。然而，即使在 x = a 處導數未定義，切線仍然可能存在。(例如，函數 <img src="{{ 'assets/img/unit1/1/19-2.png' | relative_url }}" alt="" /> 在 x = 0 有一條垂直切線)。

<br/>
<br/>
<br/>
