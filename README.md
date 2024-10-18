# <p align='center'> Applied DS Exno-03 </p>

## AIM:

To Implement Recommendation Systems using the suitable data sets.

## ALGORITHM:

STEP 1: Load the necessary Datasets.

STEP 2: Include the necessary python library.

STEP 3: Use Fuzzy library for handling text data.

STEP 4: Perform Data Preprocessing Steps.

STEP 5: Standardize column names for merging.

STEP 6: Apply fuzzy matching to find similar text data between datasets.

STEP 7: Perform Data transformation between datasets.

STEP 8: Define a recommendation score using the features of the datasets.

STEP 9: Sort the data by recommendation score.

STEP 10: Export the results to a CSV file.

## CODING :
```py
import pandas as pd
import numpy as np
pip install pandas numpy scikit-learn fuzzywuzzy python-Levenshtein

from fuzzywuzzy import process
from sklearn.preprocessing import MinMaxScaler
df = pd.read_csv('emobile.csv')
df.head()
dx = pd.read_csv('maxmobile.csv')
df.drop_duplicates(inplace=True)
dx.drop_duplicates(inplace=True)
df.columns=['Product_id','Product_Name','Rating', 'Review_Count']
dx.columns=['Product_id','Product_Name','Rating', 'Review_Count']
def match_products(name,choice,limit=1):
  results=process.extract(name,choice,limit=limit)
  return results[0][0] if results else None
dx['Matched_Product'] = dx['Product_Name'].apply(lambda x: match_products(x,df['Product_Name'].tolist()))
merged_df = pd.merge(df, dx, left_on='Product_Name',right_on='Matched_Product' ,how='inner', suffixes=('_ds1', '_ds2'))
merged_df
merged_df.columns
x=merged_df['Review_Count_ds1']+merged_df['Review_Count_ds2']
merged_df['Combined_Rating'] = (merged_df['Rating_ds1'] * merged_df['Review_Count_ds1']+merged_df['Rating_ds2'] * merged_df['Review_Count_ds2'] ) / x
merged_df['Recommendation_Score']= 0.7* merged_df['Combined_Rating'] + 0.3*merged_df['Rating_ds1']
merged_df.head()
best_products = merged_df.sort_values(by='Recommendation_Score', ascending=False)
print(best_products[['Matched_Product','Combined_Rating' ,'Recommendation_Score']].head(10))
best_products[['Matched_Product', 'Combined_Rating', 'Recommendation_Score']].to_csv('best_products.csv', index=False)
rp=pd.read_csv('best_products.csv')
rp.head()
```
## OUTPUT :
### Emobile csv:
![](./O1.png)
### Merged Dataset:
![](./o4.png)
### Recommendation Systems score:
![](./O2.png)
### Final best products csv:
![](./O3.png)


## RESULT :
Thus the program for Recommendation Systems has been executed successfully.