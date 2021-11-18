# Sara-Meziani-et-Louis-Mortreuil
Projet M2 Ekap : LearnPlatform COVID-19 Impact on Digital Learning - Sara Meziani et Louis Mortreuil

Lien kaggle : https://www.kaggle.com/c/learnplatform-covid19-impact-on-digital-learning



```python
import sqlite3
import csv 
import pandas as pd
import os
os.system('cls')
import plotly.express as px

import numpy as np 
import math
import glob
import os
import seaborn as sns
import matplotlib.pyplot as plt
import seaborn as sns


```


```python
product=pd.read_csv('D:\SVM\learnplatform-covid19-impact-on-digital-learning\products_info.csv', sep= ',')
product.head()

```

```python
districts=pd.read_csv('D:\SVM\learnplatform-covid19-impact-on-digital-learning\districts_info.csv', sep= ',', header=0)
districts.head()
```

```python
path = 'D:\SVM\learnplatform-covid19-impact-on-digital-learning\engagement_data' 
all_files = glob.glob(path + "\*.csv")

li = []

for filename in all_files:
    df = pd.read_csv(filename, index_col=None, header=0)
    district_id = filename[72:76]
    df["district_id"] = district_id
    li.append(df)
    
engagement_df = pd.concat(li)
engagement_df = engagement_df.reset_index(drop=True)
engagement_df.head()
```






```python
df['district_id']

```




