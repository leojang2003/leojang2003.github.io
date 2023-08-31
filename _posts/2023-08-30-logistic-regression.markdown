---
layout: post
title: Logistic Regression
subtitle: 
tags: []
comments: true
---

### Logistic Regression

Logistic Regression 是個 binary classification 方法

Logistic Regression 是一個 classification method，用來預測 yes or no 用的。不像 Linear Regression 是用來預測連續的數值。<br class="new">

我們知道 pumpkin 資料有一個二元的欄位 'Color'，我們使用這個 Logistic Regression 來預測南瓜的顏色會是什麼。

**定義問題**

南瓜是白色或不是白色，顏色另外有一個值 striped，在我們移除 null 時就順便移除了。

**其他 classification 方法**

- **Multinomial**, 牽涉多個 categories 如 "Orange, White, and Striped".
- **Ordinal**, 牽涉 ordered categories, 如果我們要排序我們的結果，像是南瓜如果要用大小排序 (mini,sm,med,lg,xl,xxl).

**變數不需要相依**

Logistic Regression 與 Linear Regression 剛好相反，適合有弱相依性的資料。

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

### Visualization - categorical plot

這裡我們是用 seaborn，seaborn 提供不同的種類與顏色的關係，使用 catplot 設定 color 如下。

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

我們可從圖形知道 color 與 variety 的關係

### Data pre-processing: feature and label encoding

欄位如果是 categorical 的資料的話，需要將他們編碼，有兩種 encoder 方式

{: .box-note}
所謂的 categorical 資料指的是有 name 或是 label 的資料，像是南瓜的 variety，有 'HOWDEN TYPE'、'FAIRYTALE'、'PIE TYPE'。這是給人看，不是給機器看的。所以在資料預先處理的階段，encoding 是重要的階段。encoding 可以在不遺漏資訊的情況下將 categorical 資料轉成 numerical 資料。

### Feature Encoding

feature encoding 有兩種主要的方式

1. Ordinal encoder:<br class="new">
<br class="new">
適合用在 ordinal 資料，Ordinal encoder 會替每個 category 建立一個數字，依照 category 的順序。

{: .box-note}
所謂的 ordinal 資料指的是有 ranking 的 categorical 資料，像是教育程度、成績、收入範圍。 

{: .box-note}
feature 就是 column，有時候也被稱謂 variable 或 attribute。 

```python
from sklearn.preprocessing import OrdinalEncoder

item_size_categories = [['sml', 'med', 'med-lge', 'lge', 'xlge', 'jbo', 'exjbo']]
ordinal_features = ['Item Size']
ordinal_encoder = OrdinalEncoder(categories=item_size_categories)
```

2. Categorical encoder<br class="new">
<br class="new">
適合用在 nominal 資料，nominal 資料指的是沒有順序(order/ranking)的 categorical 資料，像是顏色。使用 one-hot encoding 的方式，每個 category 都是一個 binary column，也就是值只有0/1，假設 variety 有三個值 'HOWDEN TYPE'、'FAIRYTALE'、'PIE TYPE'，這三個值就變成三個欄位，如果資料是 HOWDEN TYPE，HOWDEN TYPE 就是 1，其他兩個欄位就是 0。

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

### Label Encoding

再來我們使用 scikit-learn 的 LabelEncoder 類別，來 normalize labels 使他們的值 values 變成 0/1，這裡我們將 Color 的值從 'ORANGE'、'WHITE' 轉成 0/1。

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

現在我們已經將 feature 還有 label 做完 encoding，我們可以將 merge 到新的 DataFrame，使用 DataFrame 的 assign() 方法。

```python
encoded_pumpkins = encoded_features.assign(Color=encoded_label)
```

### Analyse relationships between variables

資料處理完後，現在我們可以分析 features 與 label 的關係來預測 model 在給 feature 時可以多準確預測 label。

要做這種分析的最好的方式就是將資料畫出來。使用 Seaborn 的 catplot，來看 Item Size、Variety、Color 在 categorical plot 的關係。我們使用編碼後的 Item Size 欄和未編碼的 Variety 欄。可以看到不同種類的南瓜兩種顏色的大小分布。

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

### Use a swarm plot

也可以使用 swarm plot 來看不同顏色南瓜，大小的分布。

```python
palette = {
0: 'orange',
1: 'wheat'
}
sns.swarmplot(x="Color", y="ord__Item Size", data=encoded_pumpkins, palette=palette)
```

### Logistic Regression 的原理

<br/>
<br/>
<br/>
