---
layout: post
title: Linear Regression & Polynomial Regression
subtitle: 
tags: ['DatetimeIndex', 'explanatory variable', 'dependent variable', 'slope', 'intercept', Correlation Coefficient', 'coefficient of determination'']
comments: true
---

### 資料準備

```python
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
from datetime import datetime

# 讀取 csv
pumpkins = pd.read_csv('D:/DataAnalysis/US-pumpkins.csv')
```

選擇 Package 欄位包含 bushel 的列，str.contains 的 case 為大小寫，regex 為是否為 regular expression

```python
pumpkins = pumpkins[pumpkins['Package'].str.contains('bushel', case=True, regex=True)]
# 過濾後剩下 415 列
```

因為有很多欄有空值，我們這裡選擇沒有 NULL 的欄位，共 6 個。

```python
columns_to_select = ['Package', 'Variety', 'City Name', 'Low Price', 'High Price', 'Date']
pumpkins = pumpkins.loc[:, columns_to_select]
```

弄外新增一個平均價格的欄位

```python
price = (pumpkins['Low Price'] + pumpkins['High Price']) / 2
```

取得月份

```python
# 回傳一個 DatetimeIndex 物件，包含 TimeStamp 物件的集合
dti = pd.DatetimeIndex(pumpkins['Date'])

month = dti.month
```

再來取得日期是一年的第幾天，範例是使用 lamda 取得

```python
# to_datetime 轉成 datetime 物件
day_of_year = pd.to_datetime(pumpkins['Date']).apply(lambda dt: (dt-datetime(dt.year,1,1)).days)
```

其實可以使用 DatetimeIndex 的 dayofyear 就好了

```python
day_of_year = dti.dayofyear
```

建立一新的 DataFrame

```python
new_pumpkins = pd.DataFrame(
    {'Month': month, 
     'DayOfYear' : day_of_year, 
     'Variety': pumpkins['Variety'], 
     'City': pumpkins['City Name'], 
     'Package': pumpkins['Package'], 
     'Low Price': pumpkins['Low Price'],
     'High Price': pumpkins['High Price'], 
     'Price': price})
```

正規化 Price 欄位

```python
# loc 的第一個參數為選擇列的條件
# loc 的第二個參數為要設定的欄 
new_pumpkins.loc[new_pumpkins['Package'].str.contains('1 1/9'), 'Price'] = price/1.1
new_pumpkins.loc[new_pumpkins['Package'].str.contains('1/2'), 'Price'] = price*2
```

### 線性回歸前置作業

我們知道線性回歸是為了畫一條線，做到以下兩件事<br class="new">
1. 顯示變數的關係<br class="new">
2. 預測<br class="new">

<br class="new">

通常使用的是 **Least-Squares Regression** 來畫這條線，所有資料距離這條線的距離平方相加，相加後的值越小越好<br class="new">
這條 Regression Line 是 Y = a + bX，X 是 explanatory variable，Y 是 dependent variable，b 是 slope，a 是 intercept。

### Correlation 相關

要知道兩個變數的相關性，可以使用變數 X, Y 之間的相關係數 **Correlation Coefficient** 來探索。好的線性回歸模型具有高相關係數(在0~1之間接近1)。  

** 使用 .corr 檢視 Correlation Coefficient

```python
new_pumpkins['Month'].corr(new_pumpkins['Price']) # -0.15
new_pumpkins['DayOfYear'].corr(new_pumpkins['Price']) # -0.17
```

使用 .corr 方法後我們可以知道 Price 與 Month/DayOfYear 的關係不大，我們推測價格與種類比較有關  

使用以下程式，將不同種類的南瓜上色

```python
ax = None
colors = ['red', 'blue', 'green', 'yellow']

# enumerate 會回傳一個 tuple，分別是(索引，實際值)
for i, var in enumerate(new_pumpkins['Variety'].unique()):    
    df = new_pumpkins[new_pumpkins['Variety']==var] 
       
    # DataFrame 的 plot 方法，預設使用 matplotlib
    ax = df.plot.scatter('DayOfYear', 'Price', ax=ax, c=colors[i], label=var)
```

將種類 groupby 後，我們可以看到價格與種類相關

```python
new_pumpkins.groupby('Variety')['Price'].mean().plot(kind='bar')
```

現在我們來看種類為 'PIE TYPE' 的南瓜

```python
pie_pumpkins = new_pumpkins[new_pumpkins['Variety']=='PIE TYPE']
pie_pumpkins.plot.scatter('DayOfYear', 'Price')

print(pie_pumpkins['DayOfYear'].corr(pie_pumpkins['Price']))
# -0.27 變得比較有關係了
```

在做訓練前，將空的資料丟掉

```python
pie_pumpkins.dropna(inplace=True)
pie_pumpkins.info
```

### Linear Regression 線性回歸

```python
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
```

我們接著將輸入值(features)，與期待的輸出(label) 分開如下，之所以要做 reshape 的關係是因為 Linear Regression 預期輸入是個二維陣列

```python
X = pie_pumpkins['DayOfYear'].to_numpy().reshape(-1,1)
y = pie_pumpkins['Price']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)
lin_reg = LinearRegression()
lin_reg.fit(X_train, y_train) # 訓練

lin_reg.coef_ # -0.0175
lin_reg.intercept_ # 21.13
```

**怎麼確定模型的正確性?**

使用 MSE 檢查預測的正確性，得到 17% 左右，不是一個好看的數字，

```python
pred = lin_reg.predict(X_test)

mse = np.sqrt(mean_squared_error(y_test, pred))
print(mse/np.mean(pred)) # 17% 
```

**coefficient of determination**  

另一個驗證模型品質方式可以使用決定係數(coefficient of determination)，分數 0~1，越高越好

```python
score = lin_reg.score(X_train, y_train) # 0.04

plt.scatter(X_test, y_test)
plt.plot(X_test, pred)
```

### 多項式回歸 Polynomial Regression

當關係無法用一個直線描述時就需要多項式回歸<br class="new">
PolynomialFeatures(2) 表示會包含所有輸入值的平方，當成一個 feature，這裡我們只有一個輸入變數 DayOfYear，所以只會有 DayOfYear^2, 如果有兩個輸入變數，新增 feature 就會是 X^2、XY、Y^2。

{: .box-note}
所謂的 n-degree polynomial 就是變數最多就是 n 次方

```python
from sklearn.preprocessing import PolynomialFeatures
from sklearn.pipeline import make_pipeline

pipeline = make_pipeline(PolynomialFeatures(2), LinearRegression())

pipeline.fit(X_train,y_train)
```

使用 polynomial regression 可以獲得較低的 MSE 和較高的 determination，但無法大量提升

### Categorical Features

非數字的 column 稱為 categorical，我們可以將 categorical column 編碼
- simple encoding
- one-hot encoding，將 variety 欄以 4 個欄取代

```python
pd.get_dummies(new_pumpkins['Variety'])
#       FAIRYTALE  MINIATURE  MIXED HEIRLOOM VARIETIES  PIE TYPE
# 70            0          0                         0         1
# 71            0          0                         0         1
# 72            0          0                         0         1
# 73            0          0                         0         1
# 74            0          0                         0         1
#         ...        ...                       ...       ...
# 1738          0          1                         0         0
# 1739          0          1                         0         0
# 1740          0          1                         0         0
# 1741          0          1                         0         0
# 1742          0          1                         0         0

# [415 rows x 4 columns]
```

使用 one-hot 編碼的 variety 欄位做線性回歸如下
```python
X = pd.get_dummies(new_pumpkins['Variety'])
y = new_pumpkins['Price']

# 其他一樣
```

** join() **

將不同的 dataframe 依照值 join 起來

```python
from sklearn.preprocessing import PolynomialFeatures
from sklearn.pipeline import make_pipeline

X = pd.get_dummies(new_pumpkins['Variety']).join(new_pumpkins['Month']).join(pd.get_dummies(new_pumpkins['City'])).join(pd.get_dummies(new_pumpkins['Package']))
y = new_pumpkins['Price']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)

pipeline = make_pipeline(PolynomialFeatures(2), LinearRegression())

pipeline.fit(X_train,y_train)

pred = pipeline.predict(X_test)

mse = np.sqrt(mean_squared_error(y_test, pred))
print(mse/np.mean(pred))

score = pipeline.score(X_train, y_train) # 0.04
print(score)
```

<br/>
<br/>
<br/>
