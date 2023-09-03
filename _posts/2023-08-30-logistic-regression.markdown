---
layout: post
title: Logistic Regression
subtitle: 
tags: []
comments: true
---

### Logistic Regression

Logistic Regression 是個 binary classification 方法。

Logistic Regression 是一個 classification method，用來預測 binary category。不像 Linear Regression 是用來預測連續的數值。<br class="new">

**定義問題**

我們知道 pumpkin 資料有一個二元的欄位 'Color'，我們來建立一個 Logistic Regression 模型來預測南瓜的顏色會是什麼。南瓜是白色或不是白色，顏色另外有一個值 striped，這個值較少，在我們移除 null 時就順便移除了。

**其他 classification 方法**

- Multinomial, 牽涉多個 categories 如 "Orange, White, Striped"。
- Ordinal, 牽涉有序的 category, 如果我們要排序我們的結果，像是南瓜如果要用大小排序(mini,sm,med,lg,xl,xxl)。

**變數不需要相依**

Logistic Regression 與 Linear Regression 剛好相反，適合有弱相依性(weak correlations)的資料。

**需要大量個乾淨資料**

Logistic Regression 的資料越多準確性越高。

### 資料清理

```python
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
from datetime import datetime

pumpkins = pd.read_csv('D:/DataAnalysis/US-pumpkins.csv')

# 選擇欄位
columns_to_select = ['City Name','Package','Variety', 'Origin','Item Size', 'Color']
pumpkins = pumpkins.loc[:, columns_to_select]

pumpkins.dropna(inplace=True)

print(pumpkins.info)
# [991 rows x 6 columns]>
```

### 視覺化 - 使用 seaborn.catplot()

這裡我們是用 seaborn，seaborn 提供不同的種類與顏色的關係，使用 catplot 描繪不同種類的顏色數量，y 軸是種類/顏色，x 軸是數量。

```python
import seaborn as sns

palette = {
'ORANGE': 'orange',
'WHITE': 'wheat',
}

sns.catplot(
data=pumpkins, y="Variety", hue="Color", kind="count",
palette=palette, 
)
```

我們可從圖形知道顏色與種類相關。

### 資料預處理: feature encoding 與 label encoding

我們南瓜的資料都是字串，我們需要將 categorical 的資料轉換成機器學習程式可以用的 numerical 資料。
我們需要將欄位的 categorical 的編碼，有兩種 encoding 方式。

{: .box-note}
所謂的 categorical 資料指的是有 name 或是 label 的資料，像是南瓜的 variety，有 'HOWDEN TYPE'、'FAIRYTALE'、'PIE TYPE'。這是給人看，不是給機器看的。所以在資料預先處理的階段，encoding 是重要的階段。encoding 可以在不遺漏資訊的情況下將 categorical 資料轉成 numerical 資料。

### Feature Encoding

{: .box-note}
feature 就是 column，有時候也被稱謂 variable 或 attribute。 

Feature Encoding 有兩種主要的方式

**1. Ordinal encoder:** 

適合用在 ordinal 資料，Ordinal encoder 會替每個 category 建立一個數字，依照 category 的順序。

{: .box-note}
所謂的 ordinal 資料指的是有 ranking 的 categorical 資料，像是教育程度、成績、收入範圍。 


```python
from sklearn.preprocessing import OrdinalEncoder

item_size_categories = [['sml', 'med', 'med-lge', 'lge', 'xlge', 'jbo', 'exjbo']]
ordinal_features = ['Item Size']
ordinal_encoder = OrdinalEncoder(categories=item_size_categories)
```

**2. Categorical encoder**

適合用在 nominal 資料，nominal 資料指的是沒有順序(order/ranking)的 categorical 資料，像是顏色。使用 one-hot encoding 的方式，每個 category 都是一個 binary 欄位，也就是值只有 0/1。假設 variety 有三個值 'HOWDEN TYPE'、'FAIRYTALE'、'PIE TYPE'，這三個值就變成三個欄位，如果資料是 HOWDEN TYPE，HOWDEN TYPE 就是 1，其他兩個欄位就是 0。

```python
from sklearn.preprocessing import OneHotEncoder

categorical_features = ['City Name', 'Package', 'Variety', 'Origin']
categorical_encoder = OneHotEncoder(sparse_output=False)
```

接下來使用 ColumnTransformer 結合多個 encoder

```python
from sklearn.compose import ColumnTransformer
    
ct = ColumnTransformer(transformers=[
    ('ord', ordinal_encoder, ordinal_features),
    ('cat', categorical_encoder, categorical_features)
    ])

ct.set_output(transform='pandas')
encoded_features = ct.fit_transform(pumpkins)
# [991 rows x 48 columns]
```

encoded_features 這個 DataFrame 從原本的 6 增加到 48 欄。<br class="new">

我們可以說 Feature Encoding 將 categorical data 的值變成多個欄位，而 Label Encoding 將 categorical data 正規化成 0/1。

### Label Encoding

再來我們使用 scikit-learn 的 LabelEncoder 類別，來正規化 label 使他們的值變成 0/1，這裡我們將 Color 的值從 'ORANGE'、'WHITE' 轉成 0/1。

{: .box-note}
Label 就這裡的意思感覺上是將值做 encoding，也許我們可以將 label encoding 理解成對 feature (column) 的 label (value) 做 encoding。 

```python
from sklearn.preprocessing import LabelEncoder

label_encoder = LabelEncoder()
encoded_label = label_encoder.fit_transform(pumpkins['Color'])

# array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
#        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
#        1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
#        ...
#        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1,
#        1])
# (991,)

```

現在我們已經將 feature 還有 label 做完 encoding，我們可以 merge 到新的 DataFrame，使用 DataFrame 的 assign() 方法。

```python
encoded_pumpkins = encoded_features.assign(Color=encoded_label)
```

### 分析變數間的關係

資料處理完後，現在我們可以分析 features 與 label 的關係，來預測模型在輸入 feature 時是否可以準確預測 label。<br class="new">

要做這種分析的最好的方式就是將資料描繪出來。使用 Seaborn 的 catplot，來看 Item Size、Variety、Color 在 categorical 圖中的關係。我們使用編碼後的 Item Size 欄和未編碼的 Variety 欄。可以看到不同種類的南瓜兩種顏色的大小分布。

```python
palette = {
'ORANGE': 'orange',
'WHITE': 'wheat',
}
pumpkins['Item Size'] = encoded_pumpkins['ord__Item Size']

g = sns.catplot(
    data=pumpkins,
    x="Item Size", y="Color", row='Variety',
    kind="box", orient="h",
    sharex=False, margin_titles=True,
    height=1.8, aspect=4, palette=palette,
)
g.set(xlabel="Item Size", ylabel="").set(xlim=(0,6))
g.set_titles(row_template="{row_name}")
```

### 使用 swarm plot

也可以使用 swarm plot 來看不同顏色南瓜，大小的分布。

```python
palette = {
0: 'orange',
1: 'wheat'
}
sns.swarmplot(x="Color", y="ord__Item Size", data=encoded_pumpkins, palette=palette)
```

### Logistic Regression 的數學原理

待補

### 實際使用 Logistic Regression

```python
from sklearn.model_selection import train_test_split
    
X = encoded_pumpkins[encoded_pumpkins.columns.difference(['Color'])]
y = encoded_pumpkins['Color']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)
```

訓練 model

```python
from sklearn.metrics import f1_score, classification_report 
from sklearn.linear_model import LogisticRegression

model = LogisticRegression()
model.fit(X_train, y_train)
predictions = model.predict(X_test)

print(classification_report(y_test, predictions))
print('Predicted labels: ', predictions)
print('F1-score: ', f1_score(y_test, predictions))

# ```output
#                    precision    recall  f1-score   support

#                 0       0.94      0.98      0.96       166
#                 1       0.85      0.67      0.75        33

#     accuracy                                0.92       199
#     macro avg           0.89      0.82      0.85       199
#     weighted avg        0.92      0.92      0.92       199

#     Predicted labels:  [0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 1 0 0 0 0 0 0 0 0 1 0 0 0 0
#     0 0 0 0 0 1 0 1 0 0 1 0 0 0 0 0 1 0 1 0 1 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0
#     1 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0 1 0
#     0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 1 0 0 0 1 1 0
#     0 0 0 0 1 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1
#     0 0 0 1 0 0 0 0 0 0 0 0 1 1]
#     F1-score:  0.7457627118644068
# ```
```

### 使用 Confusion Matrix 來理解模型的好壞

我們可以使用 Confusion Matrix 來了解到我們的 model 的運作情形。

{: .box-note}
Confusion Matrix (或稱 Error Matrix) 是個表格表示模型的 true/false - 陽性/陰性。

在 Scikit-learn, Confusion Matrix 的 Rows (axis 0) 是實際的 label，column (axis 1) 是預測的 label。<br class="new">

|       |   0   |   1   |
| :---: | :---: | :---: |
|   0   |  TN   |  FP   |
|   1   |  FN   |  TP   |


假設我們的模型被要求對南瓜進行兩個 binary category 的 classification，category '白色' 和 '非白色'。<br class="new">

如果我們的模型將一個南瓜預測為非白色，並且它實際上屬於 '非白色' category，我們稱之為 True Negative，表示為左上角的數字。<br class="new">
如果我們的模型將一個南瓜預測為白色，並且它實際上屬於 '非白色' category，我們稱之為 False Negative，表示為左下角的數字。<br class="new">
如果我們的模型將一個南瓜預測為非白色，並且它實際上屬於 '白色' category，我們稱之為 False Positive，表示為右上角的數字。<br class="new">
如果我們的模型將一個南瓜預測為白色，並且它實際上屬於 '白色' category，我們稱之為 True Positive，表示為右下角的數字。<br class="new">
正如您所猜測的，更多的 True Positive 和 True Negative，以及更少的 False Positive 和 False Negative，意味著模型的性能更好。

### Confusion Matrix 與 Precision 和 Recall 之間的關係：

{: .box-note}
Precision = True Positive / (True Positive + False Positive)= 22 /(22 + 4)= 0.8461538461538461

{: .box-note}
Recall = True Positive / True Positive + False Negative = 22 /(22 + 11)= 0.6666666666666666

讓我們借助 Confusion Matrix 對之前提到的術語進行回顧，它將 TP/TN 和 FP/FN 進行了映射：

-Precision：TP/(TP + FP) 在檢索到的實例中，相關實例的比例（例如，哪些標籤被正確標記）<br class="new">

-Recall：TP/(TP + FN) 被檢索到的相關實例的比例，無論是否被正確標記<br class="new">

-f1-score：(2 * precision * recall)/(precision + recall)，precision 和 recall 的加權平均值，最佳值為 1，最差值為 0<br class="new">

-Support：檢索到的每個標籤的出現次數 <br class="new">

-Accuracy：(TP + TN)/(TP + TN + FP + FN) 對於樣本而言，被準確預測的標籤的百分比<br class="new">

-Macro Avg：對於每個標籤，計算 unweighted mean metrics，不考慮標籤不平衡的情況<br class="new">

-加權平均（Weighted Avg）：對於每個標籤，計算mean metrics，通過按其支持度（每個標籤的真實實例數）對其進行加權，從而考慮標籤不平衡的情況<br class="new">

你能想到，如果你希望模型減少 false negative 的數量，應該關注哪個指標嗎？

### ROC Curve

```python
from sklearn.metrics import roc_curve, roc_auc_score
import matplotlib
import matplotlib.pyplot as plt
%matplotlib inline

y_scores = model.predict_proba(X_test)
fpr, tpr, thresholds = roc_curve(y_test, y_scores[:,1])

fig = plt.figure(figsize=(6, 6))
plt.plot([0, 1], [0, 1], 'k--')
plt.plot(fpr, tpr)
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('ROC Curve')
```

使用 Matplotlib, 描繪 model 的 Receiving Operating Characteristic 簡稱 ROC，ROC 曲線通常用於取得 classifier 的 output，顯示 true vs. false positives。<br class="new">

ROC 曲線通常 Y 軸是 true positive rate，X 軸是 false positive rate。因此，曲線的陡峭程度和曲線與中間線之間的間隔是重要的：你希望曲線能夠迅速上升並超過該線。在我們的情況下，一開始存在一些假陽性，然後曲線逐漸上升並正確地超過了該線。

```python
auc = roc_auc_score(y_test,y_scores[:,1])
print(auc)
```

結果是 `0.9749908725812341`. AUC 的範圍是 0 到 1 越高越好。

<br/>
<br/>
<br/>
