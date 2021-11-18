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


```python
def percentage_columns(df, col):
    
    lower_limit = []
    upper_limit = []
    
    for index, row in districts.iterrows():
        s = row[col]
        if(not pd.isnull(s)):
            lower_value = s.split(",")[0].split("[")[1]
            upper_value = s.split(",")[1].split("[")[0].strip()
            lower_limit.append(lower_value)
            upper_limit.append(upper_value)
        else:
            lower_limit.append(None)
            upper_limit.append(None)
       
    col_name1 = col + '_lower_limit'
    col_name2 = col + '_upper_limit'
    
    df[col_name1] = pd.to_numeric(lower_limit)
    df[col_name2] = pd.to_numeric(upper_limit)
    
    return df

```




```python

districts_copy = districts.copy()
for col in districts_copy.columns[3:7]:
    districts_copy = percentage_columns(districts_copy, col)
districts_copy = districts_copy.dropna()
    
def filter_state(df,col,state):
    return df[['state','district_id',col]].dropna().loc[df['state']==state]


```



```python
def display(df, state):
    col1 = 'pct_black/hispanic_upper_limit'
    col2 = 'pct_free/reduced_upper_limit'
    col3 = 'county_connections_ratio_upper_limit'
    col4 = 'pp_total_raw_upper_limit'
    
    df1 = filter_state(df, col1,state)
    df2 = filter_state(df, col2,state)
    df3 = filter_state(df, col3,state)
    df4 = filter_state(df, col4,state)

    fig, ax = plt.subplots(2,2,figsize=(20,20))
    fig.suptitle(state,fontweight='bold',fontsize=30)
    
    sns.countplot(y=col1, data=df1, ax=ax[0,0])
    ax[0,0].set_title('% of Black/Hispanic')
    ax[0,0].set_ylabel(None)
    
    sns.countplot(y=col2, data=df2, ax=ax[0,1])
    ax[0,1].set_title('% of Eligible for Free/Reduced Lunch Price')
    ax[0,1].set_ylabel(None)
    
    sns.countplot(y=col3, data=df3, ax=ax[1,0])
    ax[1,0].set_title('Connection Ratio')
    ax[1,0].set_ylabel(None)
    
    sns.countplot(y=col4, data=df4, ax=ax[1,1])
    ax[1,1].set_title('Per-pupil total expenditure')
    ax[1,1].set_ylabel(None)
    
    plt.tight_layout()
    plt.show()

```




```python
display(districts_copy,"Illinois")
display(districts_copy,"Washington")
display(districts_copy,"Wisconsin")


```




```python
fig, ax  = plt.subplots(figsize=(16, 8))
fig.suptitle('Locale Distribution', size = 20, font="Serif")
explode = (0.05, 0.05, 0.05, 0.05)
labels = list(districts.locale.value_counts().index)
sizes = districts.locale.value_counts().values
ax.pie(sizes, explode=explode,startangle=60, labels=labels,autopct='%1.0f%%', pctdistance=0.7, colors=["#d45d00","#ff9100","#eaaa00","#6d6875"])
ax.add_artist(plt.Circle((0,0),0.4,fc='white'))
plt.show()

```




```python
plt.figure(figsize=(16, 10))
sns.countplot(y='Provider/Company Name', data=product, order=product["Provider/Company Name"].value_counts().index[:10])
plt.title("Top 10 Provider/Company Names",font="Serif", size=20)
plt.show()
```






```python
engagement_df['time'] = pd.to_datetime(engagement_df['time'])
```








```python
print(product["LP ID"].nunique())
print(engagement_df["lp_id"].nunique())
```














```python
products_engagement_data = pd.merge(product, engagement_df, left_on='LP ID', right_on='lp_id')
products_engagement_data.head()
```












```python
print(districts["district_id"].nunique())
print(engagement_df["district_id"].nunique())
```



```python
engagement_df["district_id"] = engagement_df["district_id"].astype(str).astype(int)
districts_engagement_data = pd.merge(districts, engagement_df, left_on='district_id', right_on='district_id')
districts_engagement_data.head()
```



```python
def plot_time_series(df,col1,col2,col3):
    max_list = df[[col1,col2]]\
        .groupby([col1])[col2].mean()\
        .sort_values(ascending=False).index[:5].tolist()

    df = df[df[col1].isin(max_list)]\
                    .reset_index(drop=True)[[col3, col1, col2]]
    df = df.pivot_table(index=col3, columns=col1, values=col2)

    fig = px.line(df, facet_col=col1, facet_col_wrap=1, width=800, height=800)
    fig.update_layout(
                      title=(col1 + " , " + col2 + " , " + col3).title(),
                      title_x=0.39,
                      template="plotly_white",
                      paper_bgcolor='#f5f7f8',
                      font = {'family': 'Serif', 'size': 20}
                     )
    fig.show()
```



```python
plot_time_series(districts_engagement_data,"state","pct_access","time")
plot_time_series(districts_engagement_data,"state","engagement_index","time")
plot_time_series(districts_engagement_data,"locale","pct_access","time")
plot_time_series(districts_engagement_data,"locale","engagement_index","time")
```



```python

```




```python

```












