---
layout: post
title: pandas - Series
subtitle: 
tags: [pandas, series]
comments: true
---

### Series 方法定義

{: .box-note}
class pandas.Series(data=None, index=None, dtype=None, name=None, copy=None, fastpath=False)   
 
Series 是一維 ndarray (numpy.ndarray) 具有軸的標籤 (axis label)，這裡的軸標籤指的是一維資料每個索引位置除了預設的[0,1,2,...]外，可以使用參數 index 將索引指定成 label。

標籤可以不用是唯一的，但必須是可雜湊的類型，可雜湊的類型像是 int、float、str 等。Series 支援 integer-based 與 label-based 的索引。另外 ndarray 的統計方法被覆寫，用來自動排除缺的資料 (目前表示為 NaN)。  


**參數說明**

**data** : 與陣列類似的物件(包含Series)、可迭代物件、字典或純量值。    

Series 的資料來源，如果資料來源是 dict，dict 的鍵值順序會維持變成標籤順序。  

**index** : 類似陣列的物件或索引  

值必須是可雜湊的並與 data 有同樣的長度。允許使用相同索引值。預設是 RangeIndex (0, 1, 2, …, n)。如果資料是與字典類似且 index 是 None，Series 會使用 dict 的 key 作為索引. 如果 index 不是 None，則 Series 使用 index 的值來重編索引。重新編值時，如果新的索引沒有對應到字典的鍵值，則值會是NaN

**dtype** : str, numpy.dtype, or ExtensionDtype, optional  

輸出 Series 的 data type。如果沒有設定，則從 data 取得。 

**name** : Hashable, 預設 None  

給 Series 的 name。  

**copy** : bool，預設為 False  

這設定只對 Series 或一維 ndarray 輸入有效。當 copy = True 時，變更 Series 對原本 data=Series/ndarray 的資料不會影響。copy = False 時，變更 Series 對原本 data=Series/ndarray 的資料會影響。其他資料類型不論是否設定 copy，都不會影響 data 來源。

<br/>

### 如何建立 Series

**使用 dict 建立 Series**

使用 data=dict 不帶參數建立 Series

```python
d = {'a': 1, 'b': 2, 'c': 3}
ser = pd.Series(data=d)
print(ser)
# a   1
# b   2
# c   3
# dtype: int64
```

**使用 list 建立 Series**

```python
l = [1, 2]
ser = pd.Series(l)

print(ser)
# 0    	 1
# 1      2
# dtype: int64
```

**使用 ndarray 建立 Series**

```python
r = np.array([1, 2])
ser = pd.Series(r)

print(ser)
# 0    	 1
# 1      2
# dtype: int32
```

**使用 Series 建立 Series**

```python
l = [1, 2]
ser = pd.Series(l)
ser2 = pd.Series(ser)

print(ser2)
# 0    	 1
# 1      2
# dtype: int64
```

<br/>

### 使用參數 index 建立 Series 

**使用 data=dict 帶參數 index**

```python
d = {'a': 1, 'b': 2, 'c': 3}
ser = pd.Series(data=d, index=['a', 'b', 'c'])
print(ser)
# a   1
# b   2
# c   3
# dtype: int64

# 因為字典的 key 與 index 的值相同，因此 index 值沒有效果。
```

使用 data=dict 帶參數 index

```python
d = {'a': 1, 'b': 2, 'c': 3}
ser = pd.Series(data=d, index=['x', 'y', 'z'])

print(ser)
# x   NaN
# y   NaN
# z   NaN
# dtype: float64

# 一開始使用字典的 key 建立索引，接下來 Series 使用參數 index 的值重建索引(reindexing)，因為對應不到值，因此新索引的值都是 NaN。
```
 
**使用 data=list 帶參數 index**
 
data = list，值為 int，index 有值的 Series 運算

```python
s1 = pd.Series([1,2,3], index=["a","b","c"])

print(s1)
# a    1
# b    2
# c    3
# dtype: int64
```

**使用 data=ndarray 帶參數 index**
 
```python
r = np.array([1,2])
s1 = pd.Series([1,2], index=["a","b"])

print(s1)
# a    1
# b    2
# dtype: int32
```

**使用 data=Series 帶參數 index**

```python
r = np.array([1,2])
ser1 = pd.Series(r)
ser2 = pd.Series(r, index=[0,1])

print(ser2)
# 0    1
# 1    2
# dtype: int32
```
 
如果索引與 data 的索引不同，則會是 NaN
 
```python
r = np.array([1,2])
ser1 = pd.Series(r)
ser2 = pd.Series(r, index=['a','b'])

print(ser2)
# 0    NaN
# 1    NaN
# dtype: float64
```

<br/>

### 使用參數 copy 建立 Series 

**參數 copy 對 data=list 無效**

因為 data = list 的關係，即使 copy = False，Series 使用的仍是原始資料 list 的複製，因此變更 Series 不會變更原始資料 list。  

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

**參數 copy 對 data=ndarray 有效**  

我們知道 copy 的選項只對 Series 與 ndarray 有效，現在我們從一維 ndarray 建立 Series，copy 設定 False。因為 copy=False，表示 Series 使用的是原始資料非複製資料，因此原始資料被變更。

```python
r = np.array([1, 2])
ser = pd.Series(r, copy=False)
ser.iloc[0] = 999

print(r)
# array([999,   2])
# 原始 ndarray 被變更了

print(ser)
# 0    999
# 1      2
# dtype: int32
```

如果設定 copy = True，則原始 ndarray 不會被變更

```python
r = np.array([1, 2])
ser = pd.Series(r, copy=True)
ser.iloc[0] = 999

print(r)
# array([1,   2])
# 原始 ndarray 維持不變

print(ser)
# 0    999
# 1      2
# dtype: int32
```

**參數 copy 對 data=Series 有效**  

同樣的如果 Series 的 data 來源同樣是 Series，copy = False 也會修改到原始的 Series

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

{:.note} copy = False 只對 data= Series/ndarray 有用。

### Series 運算加法

Series 的運算看的是 data 的值，如果值是數字的話，則可以做 +、-、*、/、**。如果值是 str / list 則只能做 +。

**值為 list 的 Series 運算加法**

值為 list 且兩個字典有相同鍵值

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

值為 list，兩個字典只有一個鍵值相同，相加後其他鍵值都會是 NaN

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

{:.note} 除了加法之外，list 無法做其他運算

### 值為 str 的 Series 運算

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
```

str 除了 + 之外，str 無法做其他運算

```python
# TypeError: unsupported operand type(s) for -: 'str' and 'str'
# TypeError: unsupported operand type(s) for /: 'str' and 'str'
# TypeError: can't multiply sequence by non-int of type 'str'
# TypeError: unsupported operand type(s) for ** or pow(): 'str' and 'str'
```

**值為 int 的 Series 運算**

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

**值為 int，index 有值的 Series 運算**

運算時，相同 index 的值會做運算。

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

### 重編索引 (reindexing)

如果 index 設定的在 data 裡，則會交換 index 的值

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
