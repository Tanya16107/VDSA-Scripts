

```python
import pandas as pd
import numpy as np
import math
```


```python
df9 = pd.read_excel("./Cultivation/2009/2.Crop_info_op.xlsx")
df10 = pd.read_excel("./Cultivation/2010/Crop_info_op.xlsx")
df11 = pd.read_excel("./Cultivation/2011/Crop_info_op.xlsx")
df12 = pd.read_excel("./Cultivation/2012/2.Crop_info_op.xlsx")
df13 = pd.read_excel("./Cultivation/2013/2.Crop_info_op.xlsx")
df14 = pd.read_excel("./Cultivation/2014/2.Crop_info_op.xlsx")

df9.columns

```




    Index(['VDS_ID', 'PLOT_CODE', 'PLOT_AREA', 'SEASON', 'CROP', 'VAR_NAME',
           'VAR_TYPE', 'VAR_TYPE_OT', 'PRCT_AREA', 'OP_MAIN_PROD_UNIT',
           'OP_MAIN_PROD_QTY', 'OP_MAIN_PROD_RATE', 'OP_BY_PROD_UNIT',
           'OP_BY_PROD_QTY', 'OP_BY_PROD_RATE', 'OP_OT_PROD_UNIT',
           'OP_OT_PROD_QTY', 'OP_OT_PROD_RATE', 'OP_REMARKS'],
          dtype='object')




```python
df10.columns
```




    Index(['VDS_ID', 'PLOT_CODE', 'PLOT_AREA', 'SEASON', 'CROP', 'VAR_NAME',
           'VAR_TYPE', 'VAR_TYPE_OT', 'PRCT_AREA', 'OP_MAIN_PROD_UNIT',
           'OP_MAIN_PROD_QTY', 'OP_MAIN_PROD_RATE', 'OP_BY_PROD_UNIT',
           'OP_BY_PROD_QTY', 'OP_BY_PROD_RATE', 'OP_OT_PROD_UNIT',
           'OP_OT_PROD_QTY', 'OP_OT_PROD_RATE', 'OP_REMARKS'],
          dtype='object')




```python
df11.columns
```




    Index(['VDS_ID', 'PLOT_CODE', 'PLOT_AREA', 'SEASON', 'CROP', 'VAR_NAME',
           'VAR_TYPE', 'VAR_TYPE_OT', 'PRCT_AREA', 'OP_MAIN_PROD_UNIT',
           'OP_MAIN_PROD_QTY', 'OP_MAIN_PROD_RATE', 'OP_BY_PROD_UNIT',
           'OP_BY_PROD_QTY', 'OP_BY_PROD_RATE', 'OP_OT_PROD_UNIT',
           'OP_OT_PROD_QTY', 'OP_OT_PROD_RATE', 'OP_REMARKS'],
          dtype='object')




```python
df12.columns

```




    Index(['VDS_ID', 'PLOT_CODE', 'PLOT_AREA', 'SEASON', 'CROP', 'VAR_NAME',
           'VAR_TYPE', 'VAR_TYPE_OT', 'PRCT_AREA', 'OP_MAIN_PROD_UNIT',
           'OP_MAIN_PROD_QTY', 'OP_MAIN_PROD_RATE', 'OP_BY_PROD_UNIT',
           'OP_BY_PROD_QTY', 'OP_BY_PROD_RATE', 'OP_OT_PROD_UNIT',
           'OP_OT_PROD_QTY', 'OP_OT_PROD_RATE', 'OP_REMARKS'],
          dtype='object')




```python
df13.columns
```




    Index(['VDS_ID', 'PLOT_CO', 'PLOT_AREA', 'SEASON', 'CROP', 'VAR_NAME',
           'VAR_TYPE', 'VAR_TYPE_OT', 'PRCT_AREA', 'OP_MAIN_PROD_UNIT',
           'OP_MAIN_PROD_QTY', 'OP_MAIN_PROD_RATE', 'OP_BY_PROD_UNIT',
           'OP_BY_PROD_QTY', 'OP_BY_PROD_RATE', 'OP_OT_PROD_UNIT',
           'OP_OT_PROD_QTY', 'OP_OT_PROD_RATE', 'REMARKS'],
          dtype='object')




```python
df14.columns
```




    Index(['VDS_ID', 'PLOT_CO', 'PLOT_AREA', 'SEASON', 'CROP', 'VAR_NAME',
           'VAR_TYPE', 'VAR_TYPE_OT', 'PRCT_AREA', 'OP_MAIN_PROD_UNIT',
           'OP_MAIN_PROD_QTY', 'OP_MAIN_PROD_RATE', 'OP_BY_PROD_UNIT',
           'OP_BY_PROD_QTY', 'OP_BY_PROD_RATE', 'OP_OT_PROD_UNIT',
           'OP_OT_PROD_QTY', 'OP_OT_PROD_RATE', 'REMARKS'],
          dtype='object')




```python
df = pd.DataFrame(columns=['VDS_ID','Year', 'Village', 'State', 'Crop', 'Unit', 'Quantity', 'Rate'])



```


```python
i = 0
for dfs in [df9,df10,df11,df12,df13,df14]:
    for index, row in dfs.iterrows():
        vds_id = row['VDS_ID']
        
        df.loc[i, 'VDS_ID'] = vds_id
        
        df.loc[i, 'Year'] = "20" + vds_id[3:5]
        
        state = vds_id[1:3]
        
        if(state=="TS"):
            state = "AP"
            
        df.loc[i, 'State'] = state
        
        vid = vds_id[5]
        
        if(state=="MH"):
            if(vid=="A"):
                df.loc[i, 'Village'] = "Kalman"
            if(vid=="B"):
                df.loc[i, 'Village'] = "Kanzara"
            if(vid=="C"):
                df.loc[i, 'Village'] = "Kinkhed"
            if(vid=="D"):
                df.loc[i, 'Village'] = "Shirapur"
        if(state=="AP"):
            if(vid=="A"):
                df.loc[i, 'Village'] = "Aurepalle"
            if(vid=="B"):
                df.loc[i, 'Village'] = "Dokur"
            if(vid=="C"):
                df.loc[i, 'Village'] = "Janapala Cheruvu Agraharam"
            if(vid=="D"):
                df.loc[i, 'Village'] = "Pamidipadu"
        if(state=="KN"):
            if(vid=="A"):
                df.loc[i, 'Village'] = "Belladamadugu"
            if(vid=="B"):
                df.loc[i, 'Village'] = "Kapanimbargi"
            if(vid=="C"):
                df.loc[i, 'Village'] = "Markabbinahalli"
            if(vid=="D"):
                df.loc[i, 'Village'] = "Tharati"
        if(state=="GJ"):
            if(vid=="A"):
                df.loc[i, 'Village'] = "Babrol"
            if(vid=="B"):
                df.loc[i, 'Village'] = "Chatha"
            if(vid=="C"):
                df.loc[i, 'Village'] = "Karamdi Chingariya"
            if(vid=="D"):
                df.loc[i, 'Village'] = "Makhiyala"
        if(state=="MP"):                
            if(vid=="A"):
                df.loc[i, 'Village'] = "Papda"
            if(vid=="B"):
                df.loc[i, 'Village'] = "Rampura Kalan"
        
        df.loc[i,'Crop'] = row['CROP']
        df.loc[i, 'Unit'] = row['OP_MAIN_PROD_UNIT']
        df.loc[i,'Quantity'] = row['OP_MAIN_PROD_QTY']
        df.loc[i, 'Rate'] = row['OP_MAIN_PROD_RATE']
        i+=1
    
```


```python
df.to_csv("crops.csv", encoding='utf-8')
```


```python
df.shape
```




    (18149, 8)




```python

```
