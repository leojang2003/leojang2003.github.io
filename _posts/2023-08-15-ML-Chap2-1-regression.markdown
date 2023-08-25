---
layout: post
title: ML Chapter Regression 2-1 notes
subtitle: 
tags: [regression]
comments: true
---

這裡使用糖尿病的資料

```python
import matplotlib.pyplot as plt
import numpy as np
from sklearn import datasets, linear_model, model_selection

X, Y = datasets.load_diabetes(return_X_y=True)
```

這裡的 X 是一個 ndarray，這裡複習一下 ndarray 取行的方式。

**ndarray 取 column**

```python
X = X[:,2]
# 取所有列，第2行
```

**ndarray 的 shape()**

使用 shape() 可以回傳 array 的 shape (l1,l2,l3)，l1, l2, l3, ... 分別代表第 N 個維度的長度，範例如下

```python
arr = np.array(1)
print(arr.shape)
# ()
# 第一個維度跟第二個維度都沒有

arr = np.array([1])
print(arr.shape)
# (1,)
# 第一個維度長度1，沒有第二個維度

arr = np.array([[1]])
print(arr.shape)
# (1, 1)
# 第一個維度長度1，第二個維度長度1

arr = np.array([1, 2])
print(arr.shape)
# (2,)
# 第一個維度長度2，沒有第二個維度

arr = np.array([[1, 2]])
print(arr.shape)
# (1, 2)
# 第一個維度長度1，第二個維度長度2

arr = np.array([[1, 2], [3, 4]])
print(arr.shape)
# (2, 2)
# 第一個維度長度1，第二個維度長度2
```

回到我們的 X，可以看到 X 是的 shape 回傳的(422,) 表示是一個第一個維度長度 422 的 ndarray

```python
X.shape
# (442,)
```

**reshape()**

{: .box-note}
ndarray.reshape(shape, order='C')

shape 如果是數字 n 的話，就是變成一維陣列長度為 n，或者是 tuple。

```python
arr = np.array([[1, 2, 3], [4, 5, 6]])
arr.shape
# (2, 3)

arr.reshape(1)
# ValueError: cannot reshape array of size 6 into shape (1,)
# reshape 成一維陣列至少要同原陣列的長度

new = arr.reshape(6)
# array([1, 2, 3, 4, 5, 6])
```

原陣列不受影響

```python
arr.shape
# (2, 3)
# reshape 不會影響原本的陣列

new.shape
# (6,)
```

reshape 使用 tuple

```python
new = arr.reshape(-1, 1)
# array([[1],
#        [2],
#        [3],
#        [4],
#        [5],
#        [6]])

new.shape
# (6, 1)

new = arr.reshape(-1, 6)
# array([[1, 2, 3, 4, 5, 6]])

new.shape
# (1, 6)

new = arr.reshape(3, 2)
# array([[1, 2],
#        [3, 4],
#        [5, 6]])
```

回到 X ，我們要使用 reshape() 將它變成一個二維 ndarray

```python
X = X.reshape((-1,1))
# 第二個參數選擇第二個維度的長度

# array([[ 0.06169621],
#        [-0.05147406],
#        ...       
#        [ 0.03906215],
#        [-0.0730303 ]])

print(X.shape)
# (442, 1)
# 轉成二維陣列

# 試著改變第二個參數

X = X.reshape((-1,2))
# array([[ 0.06169621, -0.05147406],
#        [ 0.04445121, -0.01159501],
#        ...
#        [-0.01590626, -0.01590626],
#        [ 0.03906215, -0.0730303 ]])

X.shape
# (221, 2)
```

**train_test_split**


```python
X_train, X_test, y_train, y_test = model_selection.train_test_split(X, y, test_size=0.33)
model = linear_model.LinearRegression()
model.fit(X_train, y_train)

y_pred = model.predict(X_test)

print(y_pred)

plt.scatter(X_test, y_test, color='black')
plt.plot(X_test, y_pred, color='blue', linewidth=3)
plt.xlabel('Scaled BMIs')
plt.ylabel('Diease Progression')
plt.title('A Graph Plot Showing Diabetes Progression Against BMI')
plt.show()

# Hold-Out Validation 
# 將資料轉成 training/testing set 的過程就做 Hold-Out Validation

#%%##   

# Assignment

X, y = datasets.load_linnerud(return_X_y=True)

# X 是 data，exercise 資料 chins/Situps/Jumps
# y 是 target，生理資訊 Weight/Waist/Pulse

# 要怎麼描述腰圍與可以做幾個 Situp 的關係?

X = X[:, 1]
y = y[:, 1]

X = X.reshape((-1,1))

X_train, X_test, y_train, y_test = model_selection.train_test_split(X, y, test_size=0.33)
model = linear_model.LinearRegression()
model.fit(X_train, y_train)

y_pred = model.predict(X_test)

print(y_pred)

plt.scatter(X_test, y_test, color='black')
plt.plot(X_test, y_pred, color='blue', linewidth=3)
plt.xlabel('situps')
plt.ylabel('waist')
plt.title('A Graph Plot Showing Waist Against Situps')
plt.show()

#%%## 

import pandas as pd
pumpkins = pd.read_csv('D:/DataAnalysis/US-pumpkins.csv')

# 因為 Package 的欄位差太多了
# 我們要過濾這欄位
# 剩下 415 行
pumpkins = pumpkins[pumpkins['Package'].str.contains('bushel', case=True, regex=True)]

# 檢查是否有空的
pumpkins.isnull().sum()

# City Name             0
# Type               1712
# Package               0
# Variety               5
# Sub Variety        1461
# Grade              1757
# Date                  0
# Low Price             0
# High Price            0
# Mostly Low          103
# Mostly High         103
# Origin                3
# Origin District    1626
# Item Size           279
# Color               616
# Environment        1757
# Unit of Sale       1595
# Quality            1757
# Condition          1757
# Appearance         1757
# Storage            1757
# Crop               1757
# Repack                0
# Trans Mode         1757
# Unnamed: 24        1757
# Unnamed: 25        1654

# 只選有用到的欄位
columns_to_select = ['Package', 'Low Price', 'High Price', 'Date']
pumpkins = pumpkins.loc[:, columns_to_select]

# 新增欄位
price = (pumpkins['Low Price'] + pumpkins['High Price'])/2

month = pd.DatetimeIndex(pumpkins['Date']).month

# 重新建立 DataFrame
new_pumpkins = pd.DataFrame({'Month':month, 'Package':pumpkins['Package'], 'Low Price':pumpkins['Low Price'], 'High Price':pumpkins['High Price'], 'Price':price})

# 正規化欄位
new_pumpkins.loc[new_pumpkins['Package'].str.contains('1 1/9'), 'Price'] = price/(1 + 1/9)
new_pumpkins.loc[new_pumpkins['Package'].str.contains('1/2'), 'Price'] = price/(1/2)

import matplotlib.pyplot as plt

price = new_pumpkins.Price
month = new_pumpkins.Month
plt.scatter(price, month)
plt.show()

new_pumpkins.groupby(['Month'])['Price'].mean().plot(kind='bar')
plt.ylabel("Pumpkin Price")
```
