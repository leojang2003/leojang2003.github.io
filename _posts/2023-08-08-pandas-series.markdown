---
layout: post
title: pandas - Series
subtitle: 
tags: [pandas, series]
comments: true
---

### 官方文件的 Series 方法定義

{: .box-note}
class pandas.Series(data=None, index=None, dtype=None, name=None, copy=None, fastpath=False)   
 
Series 是一維 ndarray (numpy.ndarray) 具有 axis labels (包含 time series
，這不知道是什麼意思)。  

Labels 可以不用是唯一的，但必須是可雜湊的類型。Series 支援 integer- 與 label-based 的索引並提供大量牽涉索引的方法。另外 ndarray 的統計方法被覆寫，用來自動排除缺的資料 (目前表示為 NaN)。  


**參數說明**

**data** : 類似陣列的物件、可迭代物件、字典或純量值。    

包含儲存在 Series 的資料，如果資料是 dict，他的參數順序會保留。  

**index** : 類似陣列的物件或索引  

值必須是可雜湊的並與 data 有同樣的長度。允許使用相同索引值。預設是 RangeIndex (0, 1, 2, …, n)。如果資料是類似 dict 且 index 是 None，Series 會使用 dict 的 key 作為索引. 如果 index 不是 None，則 Series 使用 index 的值來重編索引。  

**dtype** : str, numpy.dtype, or ExtensionDtype, optional  

輸出 Series 的 data type。如果沒有設定，則從 data 取得。 

**name** : Hashable, 預設 None  

給 Series 的 name。  

**copy** : bool，預設為 False  

複製輸入資料。只影響 Series 或一維 ndarray 輸入。其他資料類型不論是否設定 copy，都是當成有 copy

<br/>

### 官方文件的 Series 範例 

資料來自 dict 且設定有 index，因為字典的 key 與 index 的值相同，因此 index 值沒有效果。

```python
d = {'a': 1, 'b': 2, 'c': 3}
ser = pd.Series(data=d, index=['a', 'b', 'c'])
print(ser)
# a   1
# b   2
# c   3
# dtype: int64
```

一開始使用字典的 key 建立索引，接下來 Series 使用參數 index 的值重建索引，因為對應不到，因此新索引的值都是 NaN。

```python
d = {'a': 1, 'b': 2, 'c': 3}
ser = pd.Series(data=d, index=['x', 'y', 'z'])

print(ser)
# x   NaN
# y   NaN
# z   NaN
# dtype: float64
 ```
<br/>

### 參數 copy

從 list 建立 Series，copy 設定 False。因為輸入資料類型的關係，即使 copy=False，Series 使用的仍是原始資料的複製，因此原始資料沒有被變更。  

```python
r = [1, 2]
ser = pd.Series(r, copy=False)
ser.iloc[0] = 999

print(r)
# [1, 2]

print(ser)
# 0    999
# 1      2
# dtype: int64
```

我們知道 copy 的選項只對 Series 與 ndarray 有效，現在我們從一維 ndarray 建立 Series，copy 設定 False。因為輸入資料類型的關係，使用 copy=False，表示 Series 使用的是原始資料非複製資料，因此原始資料被變更。

```python
r = np.array([1, 2])
ser = pd.Series(r, copy=False)
ser.iloc[0] = 999

print(r)
# array([999,   2])

print(ser)
# 0    999
# 1      2
# dtype: int64
```

同樣的如果 Series 的 data 是 Series，copy = False 也會修改到原始的 Series

```python
s1 = pd.Series([1,2,3])

print(s1)
# 0    1
# 1    2
# 2    3
# dtype: int64

s2 = pd.Series(s1, copy=False)

print(s2)
# 0    1
# 1    2
# 2    3
# dtype: int64

s2.iloc[0] = 999

print(s2)
# 0    999
# 1      2
# 2      3
# dtype: int64

print(s1)
# 0    999
# 1      2
# 2      3
# dtype: int64
```

{:.note} copy 只對 data=Series 或 data=ndarray 有用。

### data = dict 的 Series 運算加法

data 為 dict，值為 list 且有對應 key 的相加

```python
import os
import pandas as pd

dict1 = {"col":[1,2], "col2":[3,4]}
dict2 = {"col":[5,6], "col2":[7,8]}

s1 = pd.Series(dict1)
s2 = pd.Series(dict2)

print(s1)
# col     [1, 2]
# col2    [3, 4]
# dtype: object

print(s2)
# col     [5, 6]
# col2    [7, 8]
# dtype: object

print(s1+s2)
# col     [1, 2, 5, 6]
# col2    [3, 4, 7, 8]
# dtype: object
```

data = dict，值為 list 且沒有對應的 key 相加

```python
dict1 = {"col":[1,2], "col2":[3,4]}
dict2 = {"col":[5,6], "col3":[7,8]}

s1 = pd.Series(dict1)
s2 = pd.Series(dict2)

print(s1)
# col     [1, 2]
# col2    [3, 4]
# dtype: object

print(s2)
# col     [5, 6]
# col3    [7, 8]
# dtype: object

print(s1+s2)
# col     [1, 2, 5, 6]
# col2             NaN
# col3             NaN
# dtype: object
```

除了加法之外，list 無法做其他運算

### data = list，值為 str 的 Series 運算

```python
s1 = pd.Series(['a','b','c'])
s2 = pd.Series(['d','e'])

print(s1)
# 0    a
# 1    b
# 2    c
# dtype: object

print(s2)
# 0    d
# 1    e
# dtype: object

print(s1+s2)
# 0     ad
# 1     be
# 2    NaN
# dtype: object

# 除了+之外，str無法做其他運算

# TypeError: unsupported operand type(s) for -: 'str' and 'str'
# TypeError: unsupported operand type(s) for /: 'str' and 'str'
# TypeError: can't multiply sequence by non-int of type 'str'
# TypeError: unsupported operand type(s) for ** or pow(): 'str' and 'str'
```

data = list，值為 int 的 Series 運算

```python
s1 = pd.Series([1,2,3])
s2 = pd.Series([4,5])

print(s1)
# 0    1
# 1    2
# 2    3
# dtype: int64

print(s2)
# 0    4
# 1    5
# dtype: int64

print(s1+s2)
# 0    5.0
# 1    7.0
# 2    NaN
# dtype: float64

print(s1-s2)
# 0   -3.0
# 1   -3.0
# 2    NaN
# dtype: float64

print(s1*s2)
# 0     4.0
# 1    10.0
# 2     NaN
# dtype: float64

print(s1/s2)
# 0    0.25
# 1    0.40
# 2     NaN
# dtype: float64

print(s1**s2)
# 0     1.0
# 1    32.0
# 2     NaN
# dtype: float64
```

data = list，值為 int，index 有值的 Series 運算

```python
s1 = pd.Series([1,2,3], index=["a","b","c"])
s2 = pd.Series([4,6], index=["b","a"])

print(s1)
# a    1
# b    2
# c    3
# dtype: int64

print(s2)
# b    4
# a    6
# dtype: int64

print(s1+s2)
# a    7.0
# b    6.0
# c    NaN
# dtype: float64
```

<br/>

### Series 參數 dtype, name

```python
s1 = pd.Series([1,2,3], dtype=str, name='Test')

print(s1)
# 0    1
# 1    2
# 2    3
# Name: Test, dtype: object
```

如果 index 設定的在 data 裡，則會交換 index 的 value

```python
dict1 = {"col":[1,2], "col2":[3,4]}
dict2 = {"col":[5,6], "col2":[7,8]}

s1 = pd.Series(dict1, index=["col2","col"])
s2 = pd.Series(dict2, index=["a", "b"])

print(s1)
# col2    [3, 4]
# col     [1, 2]
# dtype: object

print(s2)
# a    NaN
# b    NaN
# dtype: object
```

如果 index 設定的不在 data 裡，則會變成 NaN

<br/>
<br/>
<br/>
