

```python
import pandas as pd
import numpy as np
import math
import seaborn as sns
import matplotlib.pyplot as plt
import seaborn as sns
```


```python

```


```python
df = pd.read_csv("./Cultivation_Input_2009-14.csv")
```


```python
df.shape
df.columns

```




    Index(['VDS_ID', 'YEAR', 'STATE_VILLAGE', 'PLOT_CODE', 'CROPPED_AREA',
           'OPERATION', 'WORK_HR', 'NAME_MAT', 'RATE_MAT'],
          dtype='object')




```python

```


```python

```


```python
sub = df.loc[(df['OPERATION']=="IRRIGATION")]
ax = sns.countplot(x="NAME_MAT", data=sub, order=pd.value_counts(sub['NAME_MAT']).iloc[:6].index)
plt.title("OPERATION = IRRIGATION")
plt.show()

sub = df.loc[df['OPERATION']=="PLANT PROTECTION"]
ax = sns.countplot(x="NAME_MAT", data=sub, order=pd.value_counts(sub['NAME_MAT']).iloc[:5].index)
plt.title("OPERATION = PLANT PROTECTION")
plt.show()

sub = df.loc[df['OPERATION']=="FERTILIZER APPLICATION"]
ax = sns.countplot(x="NAME_MAT", data=sub, order=pd.value_counts(sub['NAME_MAT']).iloc[:5].index)
plt.title("OPERATION = FERTILIZER APPLICATION")
plt.show()


```


![png](output_6_0.png)



![png](output_6_1.png)



![png](output_6_2.png)



```python
df[:5].dtypes

df["RATE_MAT"] = df["RATE_MAT"].apply(pd.to_numeric, errors='coerce')
df[50:60]

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>VDS_ID</th>
      <th>YEAR</th>
      <th>STATE_VILLAGE</th>
      <th>PLOT_CODE</th>
      <th>CROPPED_AREA</th>
      <th>OPERATION</th>
      <th>WORK_HR</th>
      <th>NAME_MAT</th>
      <th>RATE_MAT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>50</th>
      <td>IAP09A0226</td>
      <td>9</td>
      <td>IAPA</td>
      <td>A</td>
      <td>2.0</td>
      <td>PLANT PROTECTION</td>
      <td></td>
      <td>MONOCROTOPHOS</td>
      <td>280.0</td>
    </tr>
    <tr>
      <th>51</th>
      <td>IAP09A0226</td>
      <td>9</td>
      <td>IAPA</td>
      <td>A</td>
      <td>2.0</td>
      <td>PLANT PROTECTION</td>
      <td></td>
      <td>CONFIDOR</td>
      <td>2400.0</td>
    </tr>
    <tr>
      <th>52</th>
      <td>IAP09A0226</td>
      <td>9</td>
      <td>IAPA</td>
      <td>A</td>
      <td>2.0</td>
      <td>PLANT PROTECTION</td>
      <td></td>
      <td>SP</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>53</th>
      <td>IAP09A0226</td>
      <td>9</td>
      <td>IAPA</td>
      <td>A</td>
      <td>2.0</td>
      <td>FERTILIZER APPLICATION</td>
      <td>8</td>
      <td>20-20-0</td>
      <td>7.6</td>
    </tr>
    <tr>
      <th>54</th>
      <td>IAP09A0226</td>
      <td>9</td>
      <td>IAPA</td>
      <td>A</td>
      <td>2.0</td>
      <td>FERTILIZER APPLICATION</td>
      <td>8</td>
      <td></td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>55</th>
      <td>IAP09A0226</td>
      <td>9</td>
      <td>IAPA</td>
      <td>A</td>
      <td>2.0</td>
      <td>PLANT PROTECTION</td>
      <td>8</td>
      <td>ANTHACOL</td>
      <td>525.0</td>
    </tr>
    <tr>
      <th>56</th>
      <td>IAP09A0226</td>
      <td>9</td>
      <td>IAPA</td>
      <td>A</td>
      <td>2.0</td>
      <td>PLANT PROTECTION</td>
      <td></td>
      <td>MONOCROTOPHOS</td>
      <td>280.0</td>
    </tr>
    <tr>
      <th>57</th>
      <td>IAP09A0226</td>
      <td>9</td>
      <td>IAPA</td>
      <td>A</td>
      <td>2.0</td>
      <td>PLANT PROTECTION</td>
      <td></td>
      <td>CONFIDOR</td>
      <td>2400.0</td>
    </tr>
    <tr>
      <th>58</th>
      <td>IAP09A0226</td>
      <td>9</td>
      <td>IAPA</td>
      <td>A</td>
      <td>2.0</td>
      <td>PLANT PROTECTION</td>
      <td></td>
      <td>SP</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>59</th>
      <td>IAP09A0226</td>
      <td>9</td>
      <td>IAPA</td>
      <td>A</td>
      <td>2.0</td>
      <td>PLANT PROTECTION</td>
      <td>8</td>
      <td>ZIRAM</td>
      <td>140.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#villages = ["IAPA"]
villages = ['IAPA', "IAPB", "IAPC", "IAPD", "IGJA", "IGJB", "IGJC", "IGJD", "IKNA", "IKNB", "IKNC", "IKND", "IMHA", "IMHB", "IMHC", "IMHD", "IMPA", "IMPB", "ITSA", "ITSB"]


materials = ['DAP', 'UREA', 'SP', 'ET', 'SM']
for village in villages:
    for mat in materials:
        for yr in range(9,15):
            sub = df.loc[(df['STATE_VILLAGE']==village) & (df['NAME_MAT']==mat) & (df['YEAR']==yr) ]
            #print(yr, village+" ("+mat+")")
            #print(sub)
            try:
                ax = sns.distplot(sub['RATE_MAT'].dropna(), hist = False, kde = True,label = yr)
            except:
                ax = sns.distplot(sub['RATE_MAT'].dropna(), hist = True, kde = False,label = yr)
        subsub = df.loc[(df['STATE_VILLAGE']==village) & (df['NAME_MAT']==mat)]
        try:
            #plt.ylim(0, subsub['RATE_MAT'].max())
            print()
        except:
            print("")
        #plt.xlim(0, None)
        plt.legend(title = 'Year')
        plt.title(village+" ("+mat+")")
        plt.xlabel('Input Rate')
        plt.ylabel('Density')
        plt.show()
```

    



![png](output_8_1.png)


    



![png](output_8_3.png)


    



![png](output_8_5.png)


    No handles with labels found to put in legend.


    



![png](output_8_8.png)


    



![png](output_8_10.png)


    



![png](output_8_12.png)


    



![png](output_8_14.png)


    



![png](output_8_16.png)


    



![png](output_8_18.png)


    



![png](output_8_20.png)


    



![png](output_8_22.png)


    



![png](output_8_24.png)


    



![png](output_8_26.png)


    



![png](output_8_28.png)


    



![png](output_8_30.png)


    



![png](output_8_32.png)


    



![png](output_8_34.png)


    



![png](output_8_36.png)


    



![png](output_8_38.png)


    



![png](output_8_40.png)


    



![png](output_8_42.png)


    



![png](output_8_44.png)


    



![png](output_8_46.png)


    



![png](output_8_48.png)


    



![png](output_8_50.png)


    



![png](output_8_52.png)


    



![png](output_8_54.png)


    No handles with labels found to put in legend.


    



![png](output_8_57.png)


    



![png](output_8_59.png)


    



![png](output_8_61.png)


    



![png](output_8_63.png)


    



![png](output_8_65.png)


    



![png](output_8_67.png)


    



![png](output_8_69.png)


    No handles with labels found to put in legend.


    



![png](output_8_72.png)


    



![png](output_8_74.png)


    



![png](output_8_76.png)


    



![png](output_8_78.png)


    



![png](output_8_80.png)


    



![png](output_8_82.png)


    



![png](output_8_84.png)


    



![png](output_8_86.png)


    



![png](output_8_88.png)


    



![png](output_8_90.png)


    No handles with labels found to put in legend.


    



![png](output_8_93.png)


    



![png](output_8_95.png)


    



![png](output_8_97.png)


    



![png](output_8_99.png)


    



![png](output_8_101.png)


    No handles with labels found to put in legend.


    



![png](output_8_104.png)


    



![png](output_8_106.png)


    



![png](output_8_108.png)


    



![png](output_8_110.png)


    No handles with labels found to put in legend.


    



![png](output_8_113.png)


    No handles with labels found to put in legend.


    



![png](output_8_116.png)


    



![png](output_8_118.png)


    



![png](output_8_120.png)


    



![png](output_8_122.png)


    



![png](output_8_124.png)


    No handles with labels found to put in legend.


    



![png](output_8_127.png)


    



![png](output_8_129.png)


    



![png](output_8_131.png)


    



![png](output_8_133.png)


    



![png](output_8_135.png)


    No handles with labels found to put in legend.


    



![png](output_8_138.png)


    



![png](output_8_140.png)


    



![png](output_8_142.png)


    



![png](output_8_144.png)


    



![png](output_8_146.png)


    No handles with labels found to put in legend.


    



![png](output_8_149.png)


    



![png](output_8_151.png)


    



![png](output_8_153.png)


    



![png](output_8_155.png)


    



![png](output_8_157.png)


    No handles with labels found to put in legend.


    



![png](output_8_160.png)


    



![png](output_8_162.png)


    



![png](output_8_164.png)


    



![png](output_8_166.png)


    



![png](output_8_168.png)


    No handles with labels found to put in legend.


    



![png](output_8_171.png)


    



![png](output_8_173.png)


    



![png](output_8_175.png)


    



![png](output_8_177.png)


    



![png](output_8_179.png)


    No handles with labels found to put in legend.


    



![png](output_8_182.png)


    



![png](output_8_184.png)


    



![png](output_8_186.png)


    



![png](output_8_188.png)


    



![png](output_8_190.png)


    



![png](output_8_192.png)


    



![png](output_8_194.png)


    



![png](output_8_196.png)


    



![png](output_8_198.png)


    No handles with labels found to put in legend.


    



![png](output_8_201.png)


    



![png](output_8_203.png)


    



![png](output_8_205.png)


    



![png](output_8_207.png)


    



![png](output_8_209.png)


    No handles with labels found to put in legend.


    



![png](output_8_212.png)


    



![png](output_8_214.png)



```python
df["WORK_HR"] = df["WORK_HR"].apply(pd.to_numeric, errors='coerce')
df[:5].dtypes

```




    VDS_ID            object
    YEAR               int64
    STATE_VILLAGE     object
    PLOT_CODE         object
    CROPPED_AREA     float64
    OPERATION         object
    WORK_HR          float64
    NAME_MAT          object
    RATE_MAT         float64
    dtype: object




```python
sns.set(style="white", color_codes=True)
for mat in materials:
    sub = df.loc[(df['NAME_MAT']==mat)]
    lm = sns.lmplot(x="CROPPED_AREA", y="WORK_HR", data=sub, lowess=True, scatter=True, col="YEAR")
    lm.fig.suptitle("Labour Hours vs Cropped Area ("+mat+")")
    plt.show()
    lm = sns.lmplot(x="CROPPED_AREA", y="WORK_HR", data=sub, lowess=True, hue="YEAR", scatter=False)
    lm.fig.suptitle("Labour Hours vs Cropped Area ("+mat+")")
    plt.show()
    lm = sns.jointplot(x="CROPPED_AREA", y="WORK_HR", data=sub, kind="reg", scatter=False)
    lm.fig.suptitle("Labour Hours vs Cropped Area ("+mat+", All years)")
    plt.show()
```


![png](output_10_0.png)



![png](output_10_1.png)



![png](output_10_2.png)



![png](output_10_3.png)



![png](output_10_4.png)



![png](output_10_5.png)



![png](output_10_6.png)



![png](output_10_7.png)



![png](output_10_8.png)



![png](output_10_9.png)



![png](output_10_10.png)



![png](output_10_11.png)



![png](output_10_12.png)



![png](output_10_13.png)



![png](output_10_14.png)



```python

```
