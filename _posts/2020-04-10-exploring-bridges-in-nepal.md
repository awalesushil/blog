---
layout: 'post'
title: 'Exploratory Data Analysis: Bridges in Nepal'
tags: ['data-analysis','python']
comment: true
---

Bridges are vital infrastructures in the development process. Moreover, in a mountainous country like Nepal with many thousands of rivers and streams, bridges play a vital role in connecting roads, villages and cities and hence moving the economy.

This is my first data analysis project whereby I explore the bridges of Nepal.

### Importing libraries

```python
import pandas as pd
import re
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
%matplotlib inline

sns.set(style="darkgrid")
pd.options.display.max_columns = None

df = pd.read_csv('data.csv', index_col='S.No.')
```

### Data Preview


```python
df.head()
```


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
    .table-div {
      overflow-x:auto;
    }
</style>

<div class="table-div">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Bridge Number</th>
      <th>Bridge Name</th>
      <th>Road Name</th>
      <th>District</th>
      <th>River/Stream</th>
      <th>Chainage(Km)</th>
      <th>Length(m)</th>
      <th>Width(m)</th>
      <th>Span No</th>
      <th>Foundation Type</th>
      <th>Loading Capacity</th>
      <th>Maintenance Division</th>
      <th>Region</th>
      <th>Completion Year</th>
      <th>Cordinate</th>
      <th>Bridge Type</th>
      <th>Bridge Condition</th>
      <th>Total Score</th>
      <th>Load Restriction</th>
      <th>Construction Status</th>
      <th>Span Length(m)</th>
      <th>Data Updated Year</th>
    </tr>
    <tr>
      <th>S.No.</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>04-H001-004</td>
      <td>Ninda Khola</td>
      <td>Mahendra Rajmarga</td>
      <td>Jhapa</td>
      <td>Ninda</td>
      <td>5.46</td>
      <td>318.60</td>
      <td>RW-7.9 | CW-7</td>
      <td>12.0</td>
      <td>RCC Foundation</td>
      <td>IRC class A/AA</td>
      <td>Damak</td>
      <td>Eastern</td>
      <td>4</td>
      <td>26.6602 | 88.1072</td>
      <td>RCC T-Beam</td>
      <td>5</td>
      <td>3.6</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>26.55</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>2</th>
      <td>04-H001-003</td>
      <td>Pali</td>
      <td>Mahendra Rajmarga</td>
      <td>Jhapa</td>
      <td>Pali Khola</td>
      <td>3.44</td>
      <td>49.60</td>
      <td>RW-7.8 | CW-7</td>
      <td>3.0</td>
      <td>Well Foundation</td>
      <td>IRC class A/AA</td>
      <td>Damak</td>
      <td>Eastern</td>
      <td>3</td>
      <td>26.6505 | 88.1368</td>
      <td>RCC T-Beam</td>
      <td>7</td>
      <td>3.1</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>16.5</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>3</th>
      <td>04-H001-028</td>
      <td>Jhiljile</td>
      <td>Mahendra Rajmarga</td>
      <td>Jhapa</td>
      <td>Jhiljhile</td>
      <td>36.64</td>
      <td>5.75</td>
      <td>RW-10.6 | CW-9.8</td>
      <td>1.0</td>
      <td>Natural Rock</td>
      <td>IRC class A/AA</td>
      <td>Damak</td>
      <td>Eastern</td>
      <td>28</td>
      <td>26.6433 | 87.8071</td>
      <td>RCC Slab</td>
      <td>7</td>
      <td>3.1</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>5.75</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>4</th>
      <td>04-H001-001</td>
      <td>Mechi</td>
      <td>Mahendra Rajmarga</td>
      <td>Jhapa</td>
      <td>Mechi</td>
      <td>0.00</td>
      <td>583.00</td>
      <td>RW-7.86 | CW-7</td>
      <td>20.0</td>
      <td>RCC Foundation</td>
      <td>IRC class A/AA</td>
      <td>Damak</td>
      <td>Eastern</td>
      <td>1</td>
      <td>26.6440 | 88.1638</td>
      <td>RCC T-Beam</td>https://github.com/awalesushil/exploratory-data-analysis/blob/master/bridges/Bridges%20in%20Nepal.ipynb
      <td>6</td>
      <td>2.7</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>29.15</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>5</th>
      <td>04-H001-031</td>
      <td>Chyangri</td>
      <td>Mahendra Rajmarga</td>
      <td>Jhapa</td>
      <td>Chyangri</td>
      <td>42.25</td>
      <td>6.70</td>
      <td>RW-10.6 | CW-9.7</td>
      <td>1.0</td>
      <td>Natural Rock</td>
      <td>NaN</td>
      <td>Damak</td>
      <td>Eastern</td>
      <td>31</td>
      <td>26.6494 | 87.7619</td>
      <td>RCC Slab</td>
      <td>7</td>
      <td>3.6</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>6.7</td>
      <td>2017</td>
    </tr>
  </tbody>
</table>
</div>



### Renaming and Droping Columns


```python
df.columns
```




    Index(['Bridge Number', 'Bridge Name', 'Road Name', 'District', 'River/Stream',
           'Chainage(Km)', 'Length(m)', 'Width(m)', 'Span No', 'Foundation Type',
           'Loading Capacity', 'Maintenance Division', 'Region', 'Completion Year',
           'Cordinate', 'Bridge Type', 'Bridge Condition', 'Total Score',
           'Load Restriction', 'Construction Status', 'Span Length(m)',
           'Data Updated Year'],
          dtype='object')




```python
def format_column(col):
    col = col.lower()
    col = col.replace(")","")
    col = re.sub(r'[/(\s+]', '_', col)
    return col
```


```python
df.columns = [format_column(col) for col in df]
df.columns
```




    Index(['bridge_number', 'bridge_name', 'road_name', 'district', 'river_stream',
           'chainage_km', 'length_m', 'width_m', 'span_no', 'foundation_type',
           'loading_capacity', 'maintenance_division', 'region', 'completion_year',
           'cordinate', 'bridge_type', 'bridge_condition', 'total_score',
           'load_restriction', 'construction_status', 'span_length_m',
           'data_updated_year'],
          dtype='object')




```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 1932 entries, 1 to 1932
    Data columns (total 22 columns):
     #   Column                Non-Null Count  Dtype  
    ---  ------                --------------  -----  
     0   bridge_number         1932 non-null   object 
     1   bridge_name           1932 non-null   object 
     2   road_name             1932 non-null   object 
     3   district              1932 non-null   object 
     4   river_stream          1932 non-null   object 
     5   chainage_km           1932 non-null   float64
     6   length_m              1932 non-null   float64
     7   width_m               1932 non-null   object 
     8   span_no               1875 non-null   float64
     9   foundation_type       1721 non-null   object 
     10  loading_capacity      496 non-null    object 
     11  maintenance_division  1909 non-null   object 
     12  region                1932 non-null   object 
     13  completion_year       1932 non-null   int64  
     14  cordinate             1932 non-null   object 
     15  bridge_type           1875 non-null   object 
     16  bridge_condition      1932 non-null   int64  
     17  total_score           1932 non-null   float64
     18  load_restriction      1585 non-null   float64
     19  construction_status   1932 non-null   object 
     20  span_length_m         1875 non-null   object 
     21  data_updated_year     1932 non-null   int64  
    dtypes: float64(5), int64(3), object(14)
    memory usage: 347.2+ KB



```python
df.drop(['bridge_number','chainage_km','width_m','span_no','foundation_type','loading_capacity','maintenance_division','completion_year','cordinate','total_score','load_restriction','span_length_m','data_updated_year'], axis=1, inplace=True)
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 1932 entries, 1 to 1932
    Data columns (total 9 columns):
     #   Column               Non-Null Count  Dtype  
    ---  ------               --------------  -----  
     0   bridge_name          1932 non-null   object 
     1   road_name            1932 non-null   object 
     2   district             1932 non-null   object 
     3   river_stream         1932 non-null   object 
     4   length_m             1932 non-null   float64
     5   region               1932 non-null   object 
     6   bridge_type          1875 non-null   object 
     7   bridge_condition     1932 non-null   int64  
     8   construction_status  1932 non-null   object 
    dtypes: float64(1), int64(1), object(7)
    memory usage: 150.9+ KB


# Data Exploration

## A. Frequency Statistics

### 1. Count of bridges by region


```python
sns.countplot(x='region', data=df)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f736c28f048>




![png](../../images/bridges_graphs/output_15_1.png)


### 2. Count of bridges by type


```python
plt.figure(figsize=(16, 6))
sns.countplot(y='bridge_type', data=df)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f7369b50240>




![png](../../images/bridges_graphs/output_17_1.png)


### 3. Count of RCC T-Beam bridges by region


```python
plt.figure(figsize=(16, 6))
sns.countplot(x='region', data=df[df.bridge_type == 'RCC T-Beam'])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f7369ac9080>




![png](../../images/bridges_graphs/output_19_1.png)


### 4. Count of bridges by district


```python
plt.figure(figsize=(16, 6))
fig = sns.countplot(x='district', data=df)
plt.xticks(rotation = 65, horizontalalignment='right')
plt.show()
```


![png](../../images/bridges_graphs/output_21_0.png)



```python
df.district.value_counts()
```




    Saptari        99
    Kailali        99
    Dang           89
    Sindhuli       70
    Bardiya        66
                   ..
    Tehrathum       4
    Dadeldhura      4
    Taplejung       3
    Jumla           2
    Okhaldhunga     1
    Name: district, Length: 70, dtype: int64



**Saptari** and **Kailali** districts have the highest number of bridges i.e. *99* whereas **Okhaldhunga** district only has *1*.

### 5. Count of different types of bridges in Saptari district


```python
plt.figure(figsize=(16, 6))
sns.countplot(x='bridge_type', data=df[df.district == 'Saptari'])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f7369a4e550>




![png](../../images/bridges_graphs/output_25_1.png)


### 6. Count of bridges in my hometown, Lalitpur


```python
df[df.district == 'Lalitpur'].district.value_counts()
```




    Lalitpur    8
    Name: district, dtype: int64



### 7. Count of bridges by construction status


```python
df.construction_status.value_counts()
```




    Completed             1724
    Under Construction     208
    Name: construction_status, dtype: int64



### 8. Count of completed bridges by condition

*Condition Scale (0-10)*

* 0 - Critical Condition- Facility is Closed and Is Beyond Repair
* 1 - Critical condition - facility is closed. Study should the feasibility for repair
* 2 - Critical condition - need for repair or rehabilitation urgent. Facility should be closed until the indicated repair is completed
* 3 - Poor condition— repair or rehabilitation required immediately
* 4 - Marginal condition—potential exists for major rehabilitation
* 5 - Generally fair condition—potential exists for minor rehabilitation
* 6 - Fair condition—potential exists for major maintenance
* 7 - Generally good condition—potential exists for minor maintenance
* 8 - Good condition—no repairs needed
* 9 - New Condition
* 10 - Not Applicable


```python
plt.figure(figsize=(16, 6))
sns.countplot(x='bridge_condition', data=df[df.construction_status == 'Completed'], log=True)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f73696e0710>




![png](../../images/bridges_graphs/output_32_1.png)


## B. Descriptive Statistics

### 1. Data Info


```python
df.describe()
```




<div class="table-div">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>length_m</th>
      <th>bridge_condition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1932.000000</td>
      <td>1932.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>45.580683</td>
      <td>7.237060</td>
    </tr>
    <tr>
      <th>std</th>
      <td>76.682149</td>
      <td>1.300804</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>12.000000</td>
      <td>7.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>24.000000</td>
      <td>7.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>50.000000</td>
      <td>8.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1149.000000</td>
      <td>10.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Replacing rows with value 0 with NaN
df = df.replace(0.0, np.NaN)
df.describe()
```




<div class="table-div">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>length_m</th>
      <th>bridge_condition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1925.000000</td>
      <td>1932.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>45.746431</td>
      <td>7.237060</td>
    </tr>
    <tr>
      <th>std</th>
      <td>76.772124</td>
      <td>1.300804</td>
    </tr>
    <tr>
      <th>min</th>
      <td>5.550000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>12.000000</td>
      <td>7.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>24.000000</td>
      <td>7.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>50.000000</td>
      <td>8.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1149.000000</td>
      <td>10.000000</td>
    </tr>
  </tbody>
</table>
</div>



The average length of bridges in ~*46* meters.

The average condition of the bridges is a *7* i.e. they are in good condition.

The length of the bridges range from *5.5* m to *1149* m.

### 2. Longest and Shortest Bridge


```python
df[df.length_m == 1149]
```




<div class="table-div">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>bridge_name</th>
      <th>road_name</th>
      <th>district</th>
      <th>river_stream</th>
      <th>length_m</th>
      <th>region</th>
      <th>bridge_type</th>
      <th>bridge_condition</th>
      <th>construction_status</th>
    </tr>
    <tr>
      <th>S.No.</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>97</th>
      <td>Koshi Barrage</td>
      <td>Mahendra Rajmarga</td>
      <td>Saptari</td>
      <td>Koshi Barrage</td>
      <td>1149.0</td>
      <td>Eastern</td>
      <td>RCC T-Beam</td>
      <td>10</td>
      <td>Completed</td>
    </tr>
  </tbody>
</table>
</div>



The **Koshi Barrage** bridge in **Saptari** district is the longest one with a length of *1149* meters.


```python
df[df.length_m == 5.55]
```




<div class="table-div">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>bridge_name</th>
      <th>road_name</th>
      <th>district</th>
      <th>river_stream</th>
      <th>length_m</th>
      <th>region</th>
      <th>bridge_type</th>
      <th>bridge_condition</th>
      <th>construction_status</th>
    </tr>
    <tr>
      <th>S.No.</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1817</th>
      <td>Ankuse khola</td>
      <td>Janakpur Circumambulatory</td>
      <td>Mahottari</td>
      <td>Ankuse khola</td>
      <td>5.55</td>
      <td>Central</td>
      <td>RCC Slab</td>
      <td>7</td>
      <td>Completed</td>
    </tr>
  </tbody>
</table>
</div>



The **Ankuse Khola** bridge in **Mahottari** district is the shortest one with a length of *5.5* meters.

### 3. Total bridges with length above 100 m


```python
df[df.length_m > 100].shape[0]
```




    176



### 4. Top 10 Longest and Shortest
#### a. Top 10 Longest


```python
df.sort_values(by=['length_m'], ascending=False).head(10)
```




<div class="table-div">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>bridge_name</th>
      <th>road_name</th>
      <th>district</th>
      <th>river_stream</th>
      <th>length_m</th>
      <th>region</th>
      <th>bridge_type</th>
      <th>bridge_condition</th>
      <th>construction_status</th>
    </tr>
    <tr>
      <th>S.No.</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>97</th>
      <td>Koshi Barrage</td>
      <td>Mahendra Rajmarga</td>
      <td>Saptari</td>
      <td>Koshi Barrage</td>
      <td>1149.0</td>
      <td>Eastern</td>
      <td>RCC T-Beam</td>
      <td>10</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>75</th>
      <td>Mahuli</td>
      <td>Mahendra Rajmarga</td>
      <td>Saptari</td>
      <td>Mahuli - 1</td>
      <td>1100.0</td>
      <td>Eastern</td>
      <td>RCC Slab</td>
      <td>8</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>1013</th>
      <td>Karnali (Geruwa) Bridge</td>
      <td>Postal Highway</td>
      <td>Bardiya</td>
      <td>Geruwa Khola</td>
      <td>1015.0</td>
      <td>Mid-Western</td>
      <td>RCC T-Beam</td>
      <td>9</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>866</th>
      <td>Thulo mai</td>
      <td>Postal Highway</td>
      <td>Jhapa</td>
      <td>Thulo mai khola</td>
      <td>800.0</td>
      <td>Eastern</td>
      <td>NaN</td>
      <td>10</td>
      <td>Under Construction</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Kankai</td>
      <td>Mahendra Rajmarga</td>
      <td>Jhapa</td>
      <td>Kankai</td>
      <td>702.0</td>
      <td>Eastern</td>
      <td>RCC T-Beam</td>
      <td>7</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>123</th>
      <td>Kamala</td>
      <td>Mahendra Rajmarga</td>
      <td>Siraha</td>
      <td>Kamala</td>
      <td>640.0</td>
      <td>Eastern</td>
      <td>RCC T-Beam</td>
      <td>8</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>926</th>
      <td>Bagmati</td>
      <td>Postal Highway</td>
      <td>Rautahat</td>
      <td>Bagmati river</td>
      <td>633.0</td>
      <td>Central</td>
      <td>RCC T-Beam</td>
      <td>10</td>
      <td>Under Construction</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Ratuwa</td>
      <td>Mahendra Rajmarga</td>
      <td>Jhapa</td>
      <td>Ratuwa</td>
      <td>585.0</td>
      <td>Eastern</td>
      <td>RCC T-Beam</td>
      <td>6</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Mechi</td>
      <td>Mahendra Rajmarga</td>
      <td>Jhapa</td>
      <td>Mechi</td>
      <td>583.0</td>
      <td>Eastern</td>
      <td>RCC T-Beam</td>
      <td>6</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>868</th>
      <td>Mechi</td>
      <td>Postal Highway</td>
      <td>Jhapa</td>
      <td>Mechi Khola</td>
      <td>560.0</td>
      <td>Eastern</td>
      <td>RCC T-Beam</td>
      <td>10</td>
      <td>Under Construction</td>
    </tr>
  </tbody>
</table>
</div>



#### b. Top 10 Longest Completed 


```python
df_sorted_length = df[df.construction_status == 'Completed'].sort_values(by=['length_m'], ascending=False)
df_sorted_length.head(10)
```




<div class="table-div">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>bridge_name</th>
      <th>road_name</th>
      <th>district</th>
      <th>river_stream</th>
      <th>length_m</th>
      <th>region</th>
      <th>bridge_type</th>
      <th>bridge_condition</th>
      <th>construction_status</th>
    </tr>
    <tr>
      <th>S.No.</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>97</th>
      <td>Koshi Barrage</td>
      <td>Mahendra Rajmarga</td>
      <td>Saptari</td>
      <td>Koshi Barrage</td>
      <td>1149.00</td>
      <td>Eastern</td>
      <td>RCC T-Beam</td>
      <td>10</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>75</th>
      <td>Mahuli</td>
      <td>Mahendra Rajmarga</td>
      <td>Saptari</td>
      <td>Mahuli - 1</td>
      <td>1100.00</td>
      <td>Eastern</td>
      <td>RCC Slab</td>
      <td>8</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>1013</th>
      <td>Karnali (Geruwa) Bridge</td>
      <td>Postal Highway</td>
      <td>Bardiya</td>
      <td>Geruwa Khola</td>
      <td>1015.00</td>
      <td>Mid-Western</td>
      <td>RCC T-Beam</td>
      <td>9</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Kankai</td>
      <td>Mahendra Rajmarga</td>
      <td>Jhapa</td>
      <td>Kankai</td>
      <td>702.00</td>
      <td>Eastern</td>
      <td>RCC T-Beam</td>
      <td>7</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>123</th>
      <td>Kamala</td>
      <td>Mahendra Rajmarga</td>
      <td>Siraha</td>
      <td>Kamala</td>
      <td>640.00</td>
      <td>Eastern</td>
      <td>RCC T-Beam</td>
      <td>8</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Ratuwa</td>
      <td>Mahendra Rajmarga</td>
      <td>Jhapa</td>
      <td>Ratuwa</td>
      <td>585.00</td>
      <td>Eastern</td>
      <td>RCC T-Beam</td>
      <td>6</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Mechi</td>
      <td>Mahendra Rajmarga</td>
      <td>Jhapa</td>
      <td>Mechi</td>
      <td>583.00</td>
      <td>Eastern</td>
      <td>RCC T-Beam</td>
      <td>6</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>1019</th>
      <td>Karnali(satighat pul)</td>
      <td>Postal Highway</td>
      <td>Kailali</td>
      <td>Karnali(satighat pul)</td>
      <td>531.00</td>
      <td>Far- Western</td>
      <td>RCC T-Beam</td>
      <td>8</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>394</th>
      <td>Karnali</td>
      <td>Mahendra Rajmarga</td>
      <td>Bardiya</td>
      <td>Karnali</td>
      <td>500.00</td>
      <td>Mid-Western</td>
      <td>Cable Stayed</td>
      <td>7</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>73</th>
      <td>Balan</td>
      <td>Mahendra Rajmarga</td>
      <td>Saptari</td>
      <td>Balan</td>
      <td>479.03</td>
      <td>Eastern</td>
      <td>RCC T-Beam</td>
      <td>7</td>
      <td>Completed</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure(figsize=(16, 6))
fig = sns.barplot(x="bridge_name", y="length_m", data=df_sorted_length.head(10))
plt.xticks(rotation = 65, horizontalalignment='right')
plt.show()
```


![png](../../images/bridges_graphs/output_49_0.png)


#### c. Top 10 Shortest


```python
df_sorted_length.tail(10).sort_values(by=['length_m'])
```




<div class="table-div">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>bridge_name</th>
      <th>road_name</th>
      <th>district</th>
      <th>river_stream</th>
      <th>length_m</th>
      <th>region</th>
      <th>bridge_type</th>
      <th>bridge_condition</th>
      <th>construction_status</th>
    </tr>
    <tr>
      <th>S.No.</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1817</th>
      <td>Ankuse khola</td>
      <td>Janakpur Circumambulatory</td>
      <td>Mahottari</td>
      <td>Ankuse khola</td>
      <td>5.55</td>
      <td>Central</td>
      <td>RCC Slab</td>
      <td>7</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>1809</th>
      <td>Odigad Khola</td>
      <td>Nagma - Gamgadhi</td>
      <td>Jumla</td>
      <td>Odigad Khola</td>
      <td>5.65</td>
      <td>Mid-Western</td>
      <td>RCC Slab</td>
      <td>7</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Jhiljile</td>
      <td>Mahendra Rajmarga</td>
      <td>Jhapa</td>
      <td>Jhiljhile</td>
      <td>5.75</td>
      <td>Eastern</td>
      <td>RCC Slab</td>
      <td>7</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>738</th>
      <td>Aringale (Muru) Khola</td>
      <td>Rapti Highway</td>
      <td>Rukum</td>
      <td>Aringale (Muru) Khola</td>
      <td>5.90</td>
      <td>Mid-Western</td>
      <td>RCC Slab</td>
      <td>7</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>928</th>
      <td>Maraha</td>
      <td>Postal Highway</td>
      <td>Rautahat</td>
      <td>Maraha khola</td>
      <td>6.00</td>
      <td>Central</td>
      <td>Arch Brick Masonry</td>
      <td>7</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>1382</th>
      <td>gharamadi khola</td>
      <td>Pokhara - Baglung - Beni - Jomsom</td>
      <td>Myagdi</td>
      <td>Gharamdi Khola</td>
      <td>6.00</td>
      <td>Western</td>
      <td>RCC Slab</td>
      <td>7</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>1371</th>
      <td>Phedi Khola</td>
      <td>Pokhara - Baglung - Beni - Jomsom</td>
      <td>Kaski</td>
      <td>Phedi khola</td>
      <td>6.00</td>
      <td>Western</td>
      <td>RCC Slab</td>
      <td>7</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>1387</th>
      <td>Bhadra (Bhyaple) khola</td>
      <td>Pokhara - Baglung - Beni - Jomsom</td>
      <td>Parbat</td>
      <td>Bhadra (Bhyaple) khola</td>
      <td>6.00</td>
      <td>Western</td>
      <td>RCC Slab</td>
      <td>6</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>884</th>
      <td>Larikatta</td>
      <td>Postal Highway</td>
      <td>Morang</td>
      <td>Larikatta</td>
      <td>6.05</td>
      <td>Eastern</td>
      <td>RCC Slab</td>
      <td>6</td>
      <td>Completed</td>
    </tr>
    <tr>
      <th>1237</th>
      <td>Pathariya kulo</td>
      <td>Junga - Rajapur</td>
      <td>Kailali</td>
      <td>Pathariya kulo</td>
      <td>6.10</td>
      <td>Far- Western</td>
      <td>RCC Slab</td>
      <td>7</td>
      <td>Completed</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure(figsize=(16, 6))
fig = sns.barplot(x="bridge_name", y="length_m", data=df_sorted_length.tail(10).sort_values(by=['length_m']))
plt.xticks(rotation = 65, horizontalalignment='right')
plt.show()
```


![png](../../images/bridges_graphs/output_52_0.png)


### 5. Districts with the most longest bridges in top 100


```python
plt.figure(figsize=(16, 6))
fig = sns.countplot(x='district', data=df_sorted_length.head(100))
plt.xticks(rotation = 65, horizontalalignment='right')
plt.show()
```


![png](../../images/bridges_graphs/output_54_0.png)


### 6. Districts with the most bridges above 100m


```python
plt.figure(figsize=(16, 6))
fig = sns.countplot(x='district', data=df[df.length_m > 100])
plt.xticks(rotation = 65, horizontalalignment='right')
plt.show()
```


![png](../../images/bridges_graphs/output_56_0.png)


### 7. Road with the most bridges above 100m


```python
plt.figure(figsize=(16, 6))
fig = sns.countplot(x='road_name', data=df[df.length_m > 100])
plt.xticks(rotation = 65, horizontalalignment='right')
plt.show()
```


![png](../../images/bridges_graphs/output_58_0.png)


### 8. Bridge type with the most bridges above 100m


```python
plt.figure(figsize=(16, 6))
fig = sns.countplot(x='bridge_type', data=df[df.length_m > 100])
plt.xticks(rotation = 65, horizontalalignment='right')
plt.show()
```


![png](../../images/bridges_graphs/output_60_0.png)


### 9. Total length of all the bridges (in m)


```python
df.length_m.sum()
```




    88061.87999999999



### 10. Total length of all the bridges by district (in m)


```python
plt.figure(figsize=(16, 6))
fig = sns.barplot(x='district',y='length_m', data=df.groupby(df.district, as_index=False).sum())
plt.xticks(rotation = 65, horizontalalignment='right')
plt.show()
```

![png](../../images/bridges_graphs/output_64_0.png)


### 11. Top 10 districts with most bridge length


```python
df.groupby(df.district, as_index=False).length_m.sum().sort_values(by=['length_m'], ascending=False).head(10)
```




<div class="table-div">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>district</th>
      <th>length_m</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>58</th>
      <td>Saptari</td>
      <td>7150.47</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Jhapa</td>
      <td>6072.39</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Kailali</td>
      <td>4996.96</td>
    </tr>
    <tr>
      <th>60</th>
      <td>Sindhuli</td>
      <td>3975.85</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Dang</td>
      <td>3409.60</td>
    </tr>
    <tr>
      <th>69</th>
      <td>Udayapur</td>
      <td>2987.08</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Bardiya</td>
      <td>2967.20</td>
    </tr>
    <tr>
      <th>39</th>
      <td>Morang</td>
      <td>2607.46</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Dhanusha</td>
      <td>2583.90</td>
    </tr>
    <tr>
      <th>42</th>
      <td>Nawalparasi</td>
      <td>2376.31</td>
    </tr>
  </tbody>
</table>
</div>



### 12. Longest bridge of each district


```python
pd.set_option('display.max_rows', None)
df.loc[df.groupby('district')['length_m'].idxmax()].sort_values(by='district')[['district','bridge_name','length_m']]
```




<div class="table-div">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>district</th>
      <th>bridge_name</th>
      <th>length_m</th>
    </tr>
    <tr>
      <th>S.No.</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1123</th>
      <td>Achham</td>
      <td>Karnali</td>
      <td>200.00</td>
    </tr>
    <tr>
      <th>213</th>
      <td>Arghakhanchi</td>
      <td>Rana Sing</td>
      <td>103.20</td>
    </tr>
    <tr>
      <th>1089</th>
      <td>Baglung</td>
      <td>Kaligandaki pul</td>
      <td>114.00</td>
    </tr>
    <tr>
      <th>831</th>
      <td>Baitadi</td>
      <td>Surnaya Gad River</td>
      <td>41.00</td>
    </tr>
    <tr>
      <th>1459</th>
      <td>Bajhang</td>
      <td>Bhahuligad</td>
      <td>77.00</td>
    </tr>
    <tr>
      <th>1787</th>
      <td>Bajura</td>
      <td>Gui Gad River</td>
      <td>61.00</td>
    </tr>
    <tr>
      <th>1778</th>
      <td>Banke</td>
      <td>Rapti Khola</td>
      <td>342.00</td>
    </tr>
    <tr>
      <th>195</th>
      <td>Bara</td>
      <td>Bakeya</td>
      <td>355.59</td>
    </tr>
    <tr>
      <th>1013</th>
      <td>Bardiya</td>
      <td>Karnali (Geruwa) Bridge</td>
      <td>1015.00</td>
    </tr>
    <tr>
      <th>1648</th>
      <td>Bhaktapur</td>
      <td>Hanumante Khola</td>
      <td>45.00</td>
    </tr>
    <tr>
      <th>1061</th>
      <td>Bhojpur</td>
      <td>Arun Khola</td>
      <td>120.00</td>
    </tr>
    <tr>
      <th>208</th>
      <td>Chitawan</td>
      <td>Narayani pul</td>
      <td>420.00</td>
    </tr>
    <tr>
      <th>1803</th>
      <td>Dadeldhura</td>
      <td>Puntura Gad khola</td>
      <td>153.35</td>
    </tr>
    <tr>
      <th>810</th>
      <td>Dailekh</td>
      <td>Lohare Khola</td>
      <td>118.20</td>
    </tr>
    <tr>
      <th>1756</th>
      <td>Dang</td>
      <td>Rapti Khola</td>
      <td>404.00</td>
    </tr>
    <tr>
      <th>829</th>
      <td>Darchula</td>
      <td>Chameliya River</td>
      <td>121.80</td>
    </tr>
    <tr>
      <th>1327</th>
      <td>Dhading</td>
      <td>Trisuli River</td>
      <td>118.00</td>
    </tr>
    <tr>
      <th>675</th>
      <td>Dhankuta</td>
      <td>Tamor</td>
      <td>200.00</td>
    </tr>
    <tr>
      <th>136</th>
      <td>Dhanusha</td>
      <td>Aurahi</td>
      <td>329.00</td>
    </tr>
    <tr>
      <th>1305</th>
      <td>Dolakha</td>
      <td>Malu</td>
      <td>78.00</td>
    </tr>
    <tr>
      <th>837</th>
      <td>Doti</td>
      <td>Seti River</td>
      <td>164.20</td>
    </tr>
    <tr>
      <th>1905</th>
      <td>Gorkha</td>
      <td>Trisuli River</td>
      <td>152.25</td>
    </tr>
    <tr>
      <th>1731</th>
      <td>Gulmi</td>
      <td>Body-guard</td>
      <td>164.00</td>
    </tr>
    <tr>
      <th>658</th>
      <td>Ilam</td>
      <td>Mai Khola</td>
      <td>53.70</td>
    </tr>
    <tr>
      <th>1439</th>
      <td>Jajarkot</td>
      <td>Chedagad Khola</td>
      <td>130.00</td>
    </tr>
    <tr>
      <th>866</th>
      <td>Jhapa</td>
      <td>Thulo mai</td>
      <td>800.00</td>
    </tr>
    <tr>
      <th>788</th>
      <td>Jumla</td>
      <td>Umgad Khola</td>
      <td>7.80</td>
    </tr>
    <tr>
      <th>1019</th>
      <td>Kailali</td>
      <td>Karnali(satighat pul)</td>
      <td>531.00</td>
    </tr>
    <tr>
      <th>793</th>
      <td>Kalikot</td>
      <td>Ghatte Khola</td>
      <td>40.00</td>
    </tr>
    <tr>
      <th>471</th>
      <td>Kanchanpur</td>
      <td>Banara River</td>
      <td>240.80</td>
    </tr>
    <tr>
      <th>308</th>
      <td>Kapilbastu</td>
      <td>Banganga</td>
      <td>290.00</td>
    </tr>
    <tr>
      <th>563</th>
      <td>Kaski</td>
      <td>Seti khola pul</td>
      <td>180.00</td>
    </tr>
    <tr>
      <th>1657</th>
      <td>Kathmandu</td>
      <td>Bagmati</td>
      <td>156.05</td>
    </tr>
    <tr>
      <th>514</th>
      <td>Kavrepalanchowk</td>
      <td>Indrawati</td>
      <td>164.00</td>
    </tr>
    <tr>
      <th>1062</th>
      <td>Khotang</td>
      <td>Pankhu Khola</td>
      <td>15.00</td>
    </tr>
    <tr>
      <th>1269</th>
      <td>Lalitpur</td>
      <td>Karmanasa</td>
      <td>42.00</td>
    </tr>
    <tr>
      <th>1821</th>
      <td>Lamjung</td>
      <td>Midim Khola</td>
      <td>87.00</td>
    </tr>
    <tr>
      <th>643</th>
      <td>Mahottari</td>
      <td>Ratu</td>
      <td>270.00</td>
    </tr>
    <tr>
      <th>167</th>
      <td>Makawanpur</td>
      <td>Rapti</td>
      <td>210.00</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Morang</td>
      <td>Lohendra</td>
      <td>385.20</td>
    </tr>
    <tr>
      <th>1379</th>
      <td>Mustang</td>
      <td>Syang Khola</td>
      <td>115.00</td>
    </tr>
    <tr>
      <th>1733</th>
      <td>Myagdi</td>
      <td>Kaligandaki Khola</td>
      <td>76.40</td>
    </tr>
    <tr>
      <th>240</th>
      <td>Nawalparasi</td>
      <td>Binai Khola</td>
      <td>246.00</td>
    </tr>
    <tr>
      <th>1636</th>
      <td>Nuwakot</td>
      <td>Tadi River</td>
      <td>75.00</td>
    </tr>
    <tr>
      <th>1067</th>
      <td>Okhaldhunga</td>
      <td>Dudh Koshi</td>
      <td>120.00</td>
    </tr>
    <tr>
      <th>733</th>
      <td>Palpa</td>
      <td>Kaligandaki</td>
      <td>93.00</td>
    </tr>
    <tr>
      <th>656</th>
      <td>Panchthar</td>
      <td>Hengwa</td>
      <td>45.70</td>
    </tr>
    <tr>
      <th>1389</th>
      <td>Parbat</td>
      <td>Modi khola (Dimuwa)</td>
      <td>61.80</td>
    </tr>
    <tr>
      <th>942</th>
      <td>Parsa</td>
      <td>Shikharivas</td>
      <td>255.00</td>
    </tr>
    <tr>
      <th>1208</th>
      <td>Pyuthan</td>
      <td>jhimruk</td>
      <td>144.00</td>
    </tr>
    <tr>
      <th>1619</th>
      <td>Ramechhap</td>
      <td>Sunkoshi</td>
      <td>100.00</td>
    </tr>
    <tr>
      <th>1258</th>
      <td>Rasuwa</td>
      <td>Runga</td>
      <td>100.00</td>
    </tr>
    <tr>
      <th>926</th>
      <td>Rautahat</td>
      <td>Bagmati</td>
      <td>633.00</td>
    </tr>
    <tr>
      <th>1861</th>
      <td>Rolpa</td>
      <td>thawang Khola Bridge</td>
      <td>168.00</td>
    </tr>
    <tr>
      <th>1112</th>
      <td>Rukum</td>
      <td>Muglu Khola</td>
      <td>48.50</td>
    </tr>
    <tr>
      <th>274</th>
      <td>Rupandehi</td>
      <td>Tinau Khola</td>
      <td>226.50</td>
    </tr>
    <tr>
      <th>1761</th>
      <td>Salyan</td>
      <td>Sharada nadi</td>
      <td>104.00</td>
    </tr>
    <tr>
      <th>1483</th>
      <td>Sankhuwasabha</td>
      <td>Sawa Khola</td>
      <td>121.00</td>
    </tr>
    <tr>
      <th>97</th>
      <td>Saptari</td>
      <td>Koshi Barrage</td>
      <td>1149.00</td>
    </tr>
    <tr>
      <th>158</th>
      <td>Sarlahee</td>
      <td>Lakhandei</td>
      <td>204.55</td>
    </tr>
    <tr>
      <th>615</th>
      <td>Sindhuli</td>
      <td>Khalte/Nigule khola</td>
      <td>191.00</td>
    </tr>
    <tr>
      <th>1278</th>
      <td>Sindhupalchowk</td>
      <td>Indrawati</td>
      <td>105.83</td>
    </tr>
    <tr>
      <th>123</th>
      <td>Siraha</td>
      <td>Kamala</td>
      <td>640.00</td>
    </tr>
    <tr>
      <th>55</th>
      <td>Sunsari</td>
      <td>Budhi</td>
      <td>128.40</td>
    </tr>
    <tr>
      <th>764</th>
      <td>Surkhet</td>
      <td>Babai Khola</td>
      <td>222.20</td>
    </tr>
    <tr>
      <th>711</th>
      <td>Syangja</td>
      <td>Aarmadi Khola</td>
      <td>93.25</td>
    </tr>
    <tr>
      <th>588</th>
      <td>Tanahu</td>
      <td>Madi khola</td>
      <td>370.00</td>
    </tr>
    <tr>
      <th>653</th>
      <td>Taplejung</td>
      <td>Kaveli</td>
      <td>98.20</td>
    </tr>
    <tr>
      <th>1054</th>
      <td>Tehrathum</td>
      <td>Tamor</td>
      <td>200.00</td>
    </tr>
    <tr>
      <th>1921</th>
      <td>Udayapur</td>
      <td>Koshi Bridge</td>
      <td>261.30</td>
    </tr>
  </tbody>
</table>
</div>

**Data**

* **Source**: List of Main Bridges of SRN, harvested from Government of Nepal, Department of Roads, Road Network 
* **Link**: [http://bms.softavi.com/dashboard/guest_report_bi](http://bms.softavi.com/dashboard/guest_report_bi){:target="_blank"}
* **Year**: 2017
* **Retrieved**: April 09, 2020
* **Published**: April 10, 2020

You can find the executable Jupyter Notebook [here](https://github.com/awalesushil/exploratory-data-analysis/blob/master/bridges/Bridges%20in%20Nepal.ipynb){:target="_blank"}.
