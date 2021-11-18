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
