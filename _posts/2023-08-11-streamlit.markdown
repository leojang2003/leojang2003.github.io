---
layout: post
title: streamlit
subtitle: 
tags: [pandas, streamlit, beautifulsoup, sklearn, iris]
comments: true
---

### Streamlit

執行 streamlit run main.py

```python
# 檔案 main.py

import os
#==> 任何數據分析編程的第一件事：設定工作目錄(Working Directory)
wkDir = "c:/Users/Admin/Desktop/台新python/";   os.chdir(wkDir)
print(os.getcwd())

# Step1. 讀取 DataFrame
import pandas as pd
target = pd.read_csv(wkDir+"data.csv");

import streamlit as st
st.set_option('deprecation.showPyplotGlobalUse', False)

st.title("台新銀行: 大數據實務網站")      #-- canvas
st.sidebar.title("控制盤")               #-- sidebar

if st.sidebar.checkbox("(1) 擷取交易數據 (-->target)"):    #-- (3A) 由用戶於控制盤勾選後,再進行數據讀取
    print("\n\n>>>>> (1) 擷取交易數據 (-->target) -----")  #-- 偵錯用
    target = pd.read_csv(wkDir+"target.csv")                  ##== (D6) 讀取數據框
    st.sidebar.write("* 交易數據檔 = target.csv")          #-- (3B) 以下為 控制盤(sidebar)設計
    st.sidebar.write("* 交易記錄數 = ", target.shape[0])   
    st.header("(1) 擷取交易數據檔(-->target)")             #-- (3B) 以下為 主畫面(content)設計
    st.write("* 交易記錄數 = ", target.shape[0])  
    st.dataframe(target.head(3))

if st.sidebar.checkbox("(2) 產生交易週時模型"):       #-- (A) 由用戶於控制盤勾選後,再進行數據讀取
    print("\n\n>>>>> (2) 產生交易週時模型 -----")     #-- 偵錯用 
    XXX["hour"] = pd.to_datetime(XXX["datetime"]).dt.hour           ##== (G2-1).(KDD3) 產生週/時標籤 (hour,weekday)
    XXX["weekday"] = pd.to_datetime(XXX["datetime"]).dt.weekday    
    TTT = pd.crosstab( XXX["hour"], XXX["weekday"], margins=True ); print(TTT[0:3])  ##== (G2-2).(KDD4) 建立 交易週時模型 (TTT)
    st.header("(2) 交易週時模型(XXX[hour,weekday])")  #-- (B) 以下為 主畫面(content)設計
    st.write("(2A) (KDD3) 產生的數據標籤-- ")  
    st.dataframe(XXX.head(3))
    st.write("(2B) (KDD4) 交易週時模型-- ")  
    st.dataframe(TTT) 
```
 ### sklearn
 
```pthon
##== sklearn/datasets: 提供了一些數據生成器和數據加載工具, 共有24個數據集
from sklearn import datasets
iris = datasets.load_iris()
print(iris.feature_names)   #-- ['sepal length (cm)', 'sepal width (cm)', 'petal length (cm)', 'petal width (cm)']
print(iris.target_names)    #-- ['setosa' 'versicolor' 'virginica']
print(iris.data.shape)      #-- (150,4)
print(iris.data[0:2,])      #-- [[5.1 3.5 1.4 0.2] [4.9 3.  1.4 0.2]]
```

### 寫入 csv

```pthon
ds = pd.read_csv(wkDir+"data_source.csv")
ds.to_excel("new.xlsx")
```

### opensource 抓資料

```pthon
URL = 'https://tcgbusfs.blob.core.windows.net/blobyoubike/YouBikeTP.json'
response = requests.get(URL)
jsonData = response.json()
data = list(jsonData["retVal"].values());
```
<br/>
<br/>
<br/>
