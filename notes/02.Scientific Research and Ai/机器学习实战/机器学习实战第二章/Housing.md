

# 回归任务——预测住房价格

 用到了线性回归、决策树以及随机森林等各种算法

# 导入数据



```python
print("Hello world!")
```

    Hello world!



```python
import os
import tarfile
from six.moves import urllib

DOWNLOAD_ROOT = "https://raw.githubusercontent.com/ageron/handson-ml/master/"
HOUSING_PATH = "datasets/housing"
HOUSING_URL = DOWNLOAD_ROOT + HOUSING_PATH + "/housing.tgz"

def fetch_housing_data(housing_url=HOUSING_URL, housing_path=HOUSING_PATH):
    if not os.path.isdir(housing_path):
        os.makedirs(housing_path)
    tgz_path = os.path.join(housing_path, "housing.tgz")
    urllib.request.urlretrieve(housing_url, tgz_path)
    housing_tgz = tarfile.open(tgz_path)
    housing_tgz.extractall(path=housing_path)
    housing_tgz.close()
```


```python
import pandas as pd

def load_housing_data(housing_path=HOUSING_PATH):
    csv_path = os.path.join(housing_path, "housing.csv")
    return pd.read_csv(csv_path)
```


```python
# fetch_housing_data()
```

# 简单查看数据


```python
housing = load_housing_data( )

# a=['经度','纬度','房屋年龄中位数','全部房屋','全部卧室','人口','家庭','收入中位数','房价中位数','海岸距离']
# housing.columns = a
housing.head()
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
      <th>longitude</th>
      <th>latitude</th>
      <th>housing_median_age</th>
      <th>total_rooms</th>
      <th>total_bedrooms</th>
      <th>population</th>
      <th>households</th>
      <th>median_income</th>
      <th>median_house_value</th>
      <th>ocean_proximity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-122.23</td>
      <td>37.88</td>
      <td>41.0</td>
      <td>880.0</td>
      <td>129.0</td>
      <td>322.0</td>
      <td>126.0</td>
      <td>8.3252</td>
      <td>452600.0</td>
      <td>NEAR BAY</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-122.22</td>
      <td>37.86</td>
      <td>21.0</td>
      <td>7099.0</td>
      <td>1106.0</td>
      <td>2401.0</td>
      <td>1138.0</td>
      <td>8.3014</td>
      <td>358500.0</td>
      <td>NEAR BAY</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-122.24</td>
      <td>37.85</td>
      <td>52.0</td>
      <td>1467.0</td>
      <td>190.0</td>
      <td>496.0</td>
      <td>177.0</td>
      <td>7.2574</td>
      <td>352100.0</td>
      <td>NEAR BAY</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-122.25</td>
      <td>37.85</td>
      <td>52.0</td>
      <td>1274.0</td>
      <td>235.0</td>
      <td>558.0</td>
      <td>219.0</td>
      <td>5.6431</td>
      <td>341300.0</td>
      <td>NEAR BAY</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-122.25</td>
      <td>37.85</td>
      <td>52.0</td>
      <td>1627.0</td>
      <td>280.0</td>
      <td>565.0</td>
      <td>259.0</td>
      <td>3.8462</td>
      <td>342200.0</td>
      <td>NEAR BAY</td>
    </tr>
  </tbody>
</table>
</div>




```python
housing.info( )
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 20640 entries, 0 to 20639
    Data columns (total 10 columns):
     #   Column              Non-Null Count  Dtype  
    ---  ------              --------------  -----  
     0   longitude           20640 non-null  float64
     1   latitude            20640 non-null  float64
     2   housing_median_age  20640 non-null  float64
     3   total_rooms         20640 non-null  float64
     4   total_bedrooms      20433 non-null  float64
     5   population          20640 non-null  float64
     6   households          20640 non-null  float64
     7   median_income       20640 non-null  float64
     8   median_house_value  20640 non-null  float64
     9   ocean_proximity     20640 non-null  object 
    dtypes: float64(9), object(1)
    memory usage: 1.6+ MB



```python
housing["ocean_proximity"].value_counts()
```




    <1H OCEAN     9136
    INLAND        6551
    NEAR OCEAN    2658
    NEAR BAY      2290
    ISLAND           5
    Name: ocean_proximity, dtype: int64




```python
housing.describe()
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
      <th>longitude</th>
      <th>latitude</th>
      <th>housing_median_age</th>
      <th>total_rooms</th>
      <th>total_bedrooms</th>
      <th>population</th>
      <th>households</th>
      <th>median_income</th>
      <th>median_house_value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>20640.000000</td>
      <td>20640.000000</td>
      <td>20640.000000</td>
      <td>20640.000000</td>
      <td>20433.000000</td>
      <td>20640.000000</td>
      <td>20640.000000</td>
      <td>20640.000000</td>
      <td>20640.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>-119.569704</td>
      <td>35.631861</td>
      <td>28.639486</td>
      <td>2635.763081</td>
      <td>537.870553</td>
      <td>1425.476744</td>
      <td>499.539680</td>
      <td>3.870671</td>
      <td>206855.816909</td>
    </tr>
    <tr>
      <th>std</th>
      <td>2.003532</td>
      <td>2.135952</td>
      <td>12.585558</td>
      <td>2181.615252</td>
      <td>421.385070</td>
      <td>1132.462122</td>
      <td>382.329753</td>
      <td>1.899822</td>
      <td>115395.615874</td>
    </tr>
    <tr>
      <th>min</th>
      <td>-124.350000</td>
      <td>32.540000</td>
      <td>1.000000</td>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>0.499900</td>
      <td>14999.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>-121.800000</td>
      <td>33.930000</td>
      <td>18.000000</td>
      <td>1447.750000</td>
      <td>296.000000</td>
      <td>787.000000</td>
      <td>280.000000</td>
      <td>2.563400</td>
      <td>119600.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>-118.490000</td>
      <td>34.260000</td>
      <td>29.000000</td>
      <td>2127.000000</td>
      <td>435.000000</td>
      <td>1166.000000</td>
      <td>409.000000</td>
      <td>3.534800</td>
      <td>179700.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>-118.010000</td>
      <td>37.710000</td>
      <td>37.000000</td>
      <td>3148.000000</td>
      <td>647.000000</td>
      <td>1725.000000</td>
      <td>605.000000</td>
      <td>4.743250</td>
      <td>264725.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>-114.310000</td>
      <td>41.950000</td>
      <td>52.000000</td>
      <td>39320.000000</td>
      <td>6445.000000</td>
      <td>35682.000000</td>
      <td>6082.000000</td>
      <td>15.000100</td>
      <td>500001.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
%matplotlib inline   
import matplotlib.pyplot as plt
housing.hist(bins=50, figsize=(20,15))
plt.show()
```


![png](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210181449924.png)
​    


# 划分测试集与训练集

## 方法一


```python
import numpy as np

def split_train_test(data, test_ratio):
    shuffled_indices = np.random.permutation(len(data))
    test_set_size = int(len(data) * test_ratio)
#     test_ratioc传入的是测试集占数据集的百分比
#     print(test_set_size)
    test_indices = shuffled_indices[:test_set_size]
    train_indices = shuffled_indices[test_set_size:]
    return data.iloc[train_indices], data.iloc[test_indices]
```


```python
train_set, test_set = split_train_test(housing, 0.2)
```


```python
print(len(train_set), "train +", len(test_set), "test")
```

    16512 train + 4128 test


## 方法二


```python
from zlib import crc32  

def test_set_check(identifier, test_ratio):     
    return crc32(np.int64(identifier)) & 0xffffffff < test_ratio *2**32  

def split_train_test_by_id(data, test_ratio, id_column):     
    ids = data[id_column]     
    in_test_set = ids.apply(lambda id_: test_set_check(id_, test_ratio))     
    return data.loc[~in_test_set], data.loc[in_test_set]
```


```python
housing_with_id = housing.reset_index()   # adds an ìndex` column 
train_set, test_set = split_train_test_by_id(housing_with_id, 0.2, "index") 
```


```python
housing[housing.isnull().T.any()]
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
      <th>longitude</th>
      <th>latitude</th>
      <th>housing_median_age</th>
      <th>total_rooms</th>
      <th>total_bedrooms</th>
      <th>population</th>
      <th>households</th>
      <th>median_income</th>
      <th>median_house_value</th>
      <th>ocean_proximity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>290</th>
      <td>-122.16</td>
      <td>37.77</td>
      <td>47.0</td>
      <td>1256.0</td>
      <td>NaN</td>
      <td>570.0</td>
      <td>218.0</td>
      <td>4.3750</td>
      <td>161900.0</td>
      <td>NEAR BAY</td>
    </tr>
    <tr>
      <th>341</th>
      <td>-122.17</td>
      <td>37.75</td>
      <td>38.0</td>
      <td>992.0</td>
      <td>NaN</td>
      <td>732.0</td>
      <td>259.0</td>
      <td>1.6196</td>
      <td>85100.0</td>
      <td>NEAR BAY</td>
    </tr>
    <tr>
      <th>538</th>
      <td>-122.28</td>
      <td>37.78</td>
      <td>29.0</td>
      <td>5154.0</td>
      <td>NaN</td>
      <td>3741.0</td>
      <td>1273.0</td>
      <td>2.5762</td>
      <td>173400.0</td>
      <td>NEAR BAY</td>
    </tr>
    <tr>
      <th>563</th>
      <td>-122.24</td>
      <td>37.75</td>
      <td>45.0</td>
      <td>891.0</td>
      <td>NaN</td>
      <td>384.0</td>
      <td>146.0</td>
      <td>4.9489</td>
      <td>247100.0</td>
      <td>NEAR BAY</td>
    </tr>
    <tr>
      <th>696</th>
      <td>-122.10</td>
      <td>37.69</td>
      <td>41.0</td>
      <td>746.0</td>
      <td>NaN</td>
      <td>387.0</td>
      <td>161.0</td>
      <td>3.9063</td>
      <td>178400.0</td>
      <td>NEAR BAY</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>20267</th>
      <td>-119.19</td>
      <td>34.20</td>
      <td>18.0</td>
      <td>3620.0</td>
      <td>NaN</td>
      <td>3171.0</td>
      <td>779.0</td>
      <td>3.3409</td>
      <td>220500.0</td>
      <td>NEAR OCEAN</td>
    </tr>
    <tr>
      <th>20268</th>
      <td>-119.18</td>
      <td>34.19</td>
      <td>19.0</td>
      <td>2393.0</td>
      <td>NaN</td>
      <td>1938.0</td>
      <td>762.0</td>
      <td>1.6953</td>
      <td>167400.0</td>
      <td>NEAR OCEAN</td>
    </tr>
    <tr>
      <th>20372</th>
      <td>-118.88</td>
      <td>34.17</td>
      <td>15.0</td>
      <td>4260.0</td>
      <td>NaN</td>
      <td>1701.0</td>
      <td>669.0</td>
      <td>5.1033</td>
      <td>410700.0</td>
      <td>&lt;1H OCEAN</td>
    </tr>
    <tr>
      <th>20460</th>
      <td>-118.75</td>
      <td>34.29</td>
      <td>17.0</td>
      <td>5512.0</td>
      <td>NaN</td>
      <td>2734.0</td>
      <td>814.0</td>
      <td>6.6073</td>
      <td>258100.0</td>
      <td>&lt;1H OCEAN</td>
    </tr>
    <tr>
      <th>20484</th>
      <td>-118.72</td>
      <td>34.28</td>
      <td>17.0</td>
      <td>3051.0</td>
      <td>NaN</td>
      <td>1705.0</td>
      <td>495.0</td>
      <td>5.7376</td>
      <td>218600.0</td>
      <td>&lt;1H OCEAN</td>
    </tr>
  </tbody>
</table>
<p>207 rows × 10 columns</p>
</div>




```python
housing_with_id["id"] = housing["longitude"] * 1000 + housing["latitude"] 
train_set, test_set = split_train_test_by_id(housing_with_id, 0.2, "id") 
```


```python
import hashlib

def test_set_check(identifier, test_ratio, hash):
    return hash(np.int64(identifier)).digest()[-1] < 256 * test_ratio

def split_train_test_by_id(data, test_ratio, id_column, hash=hashlib.md5):
    ids = data[id_column]
    in_test_set = ids.apply(lambda id_: test_set_check(id_, test_ratio, hash))
    return data.loc[~in_test_set], data.loc[in_test_set]
```


```python
housing_with_id = housing.reset_index()   
train_set, test_set = split_train_test_by_id(housing_with_id, 0.2, "index")
```


```python
housing_with_id["id"] = housing["longitude"] * 1000 + housing["latitude"]
train_set, test_set = split_train_test_by_id(housing_with_id, 0.2, "id")
```

## train_test_split()方法


```python
from sklearn.model_selection import train_test_split

train_set, test_set = train_test_split(housing, test_size=0.2, random_state=42)
```


```python
train_set.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 16512 entries, 14196 to 15795
    Data columns (total 10 columns):
     #   Column              Non-Null Count  Dtype  
    ---  ------              --------------  -----  
     0   longitude           16512 non-null  float64
     1   latitude            16512 non-null  float64
     2   housing_median_age  16512 non-null  float64
     3   total_rooms         16512 non-null  float64
     4   total_bedrooms      16512 non-null  float64
     5   population          16512 non-null  float64
     6   households          16512 non-null  float64
     7   median_income       16512 non-null  float64
     8   median_house_value  16512 non-null  float64
     9   ocean_proximity     16512 non-null  object 
    dtypes: float64(9), object(1)
    memory usage: 1.4+ MB

## 根据收入类别分层抽样划分

这里划分的目的是为了使得在测试集与训练集中，不同收入人群所占据的比例都是和总体数据中不同人群所占据的比例是相同的


```python
housing
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
      <th>longitude</th>
      <th>latitude</th>
      <th>housing_median_age</th>
      <th>total_rooms</th>
      <th>total_bedrooms</th>
      <th>population</th>
      <th>households</th>
      <th>median_income</th>
      <th>median_house_value</th>
      <th>ocean_proximity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-122.23</td>
      <td>37.88</td>
      <td>41.0</td>
      <td>880.0</td>
      <td>129.0</td>
      <td>322.0</td>
      <td>126.0</td>
      <td>8.3252</td>
      <td>452600.0</td>
      <td>NEAR BAY</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-122.22</td>
      <td>37.86</td>
      <td>21.0</td>
      <td>7099.0</td>
      <td>1106.0</td>
      <td>2401.0</td>
      <td>1138.0</td>
      <td>8.3014</td>
      <td>358500.0</td>
      <td>NEAR BAY</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-122.24</td>
      <td>37.85</td>
      <td>52.0</td>
      <td>1467.0</td>
      <td>190.0</td>
      <td>496.0</td>
      <td>177.0</td>
      <td>7.2574</td>
      <td>352100.0</td>
      <td>NEAR BAY</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-122.25</td>
      <td>37.85</td>
      <td>52.0</td>
      <td>1274.0</td>
      <td>235.0</td>
      <td>558.0</td>
      <td>219.0</td>
      <td>5.6431</td>
      <td>341300.0</td>
      <td>NEAR BAY</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-122.25</td>
      <td>37.85</td>
      <td>52.0</td>
      <td>1627.0</td>
      <td>280.0</td>
      <td>565.0</td>
      <td>259.0</td>
      <td>3.8462</td>
      <td>342200.0</td>
      <td>NEAR BAY</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>20635</th>
      <td>-121.09</td>
      <td>39.48</td>
      <td>25.0</td>
      <td>1665.0</td>
      <td>374.0</td>
      <td>845.0</td>
      <td>330.0</td>
      <td>1.5603</td>
      <td>78100.0</td>
      <td>INLAND</td>
    </tr>
    <tr>
      <th>20636</th>
      <td>-121.21</td>
      <td>39.49</td>
      <td>18.0</td>
      <td>697.0</td>
      <td>150.0</td>
      <td>356.0</td>
      <td>114.0</td>
      <td>2.5568</td>
      <td>77100.0</td>
      <td>INLAND</td>
    </tr>
    <tr>
      <th>20637</th>
      <td>-121.22</td>
      <td>39.43</td>
      <td>17.0</td>
      <td>2254.0</td>
      <td>485.0</td>
      <td>1007.0</td>
      <td>433.0</td>
      <td>1.7000</td>
      <td>92300.0</td>
      <td>INLAND</td>
    </tr>
    <tr>
      <th>20638</th>
      <td>-121.32</td>
      <td>39.43</td>
      <td>18.0</td>
      <td>1860.0</td>
      <td>409.0</td>
      <td>741.0</td>
      <td>349.0</td>
      <td>1.8672</td>
      <td>84700.0</td>
      <td>INLAND</td>
    </tr>
    <tr>
      <th>20639</th>
      <td>-121.24</td>
      <td>39.37</td>
      <td>16.0</td>
      <td>2785.0</td>
      <td>616.0</td>
      <td>1387.0</td>
      <td>530.0</td>
      <td>2.3886</td>
      <td>89400.0</td>
      <td>INLAND</td>
    </tr>
  </tbody>
</table>
<p>20640 rows × 10 columns</p>
</div>



### 函数测试


```python
housing["income_cat"] = pd.cut(housing["median_income"],                             
                               bins=[0., 1.5, 3.0, 4.5, 5.0, 6.5, 7.0, 7.5, 8., np.inf],                           
                               labels=[1, 2, 3, 4, 5, 6,7, 8, 9])
```


```python
housing["income_cat"].hist(rwidth=0.2)
```




    <AxesSubplot:>




​    
![png](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210181449925.png)
​    


### 进行分类


```python
housing["income_cat"] = pd.cut(housing["median_income"],                             
                               bins=[0., 1.5, 3.0, 4.5, 6., np.inf],                           
                               labels=[1, 2, 3, 4, 5])
```


```python
housing["income_cat"].hist()
```




    <AxesSubplot:>




​    
![png](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210181449926.png)
​    



```python
# housing["income_cat"] = np.ceil(housing["median_income"] / 1.5)
# housing["income_cat"].where(housing["income_cat"] < 5, 5.0, inplace=True)
```

### 针对类别进行分层抽样


```python
from sklearn.model_selection import StratifiedShuffleSplit

split = StratifiedShuffleSplit(n_splits=1, test_size=0.2, random_state=42)

for train_index, test_index in split.split(housing, housing["income_cat"]):
    strat_train_set = housing.loc[train_index]
    strat_test_set = housing.loc[test_index]
```

### 测试集中的收入类别比例分布


```python
 housing["income_cat"].value_counts() / len(housing)
```




    3    0.350581
    2    0.318847
    4    0.176308
    5    0.114438
    1    0.039826
    Name: income_cat, dtype: float64



# 删除income_cat属性，还原原始数据


```python
for set_ in (strat_train_set, strat_test_set):     
    set_.drop("income_cat", axis=1, inplace=True)
```

## 创建训练集副本


```python
housing = strat_train_set.copy()
```

### 地理数据可视化


```python
housing.plot(kind="scatter", x="longitude", y="latitude")
```




    <AxesSubplot:xlabel='longitude', ylabel='latitude'>




​    
![png](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210181449927.png)
​    



```python
housing.plot(kind="kde", x="longitude", y="latitude")
```




    <AxesSubplot:ylabel='Density'>




​    
![png](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210181449928.png)
​    



```python
housing.plot(kind="scatter", x="longitude", y="latitude", alpha=0.1)
```




    <AxesSubplot:xlabel='longitude', ylabel='latitude'>




​    
![png](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210181449929.png)
​    



```python
housing.plot(kind="scatter", x="longitude", y="latitude", alpha=0.4,
    s=housing["population"]/100, label="population",
    c="median_house_value", cmap=plt.get_cmap("jet"), colorbar=True,
)
plt.legend()
```




    <matplotlib.legend.Legend at 0x1e0e560d3a0>




​    
![png](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210181449930.png)
​    



```python
housing.plot(kind="scatter", x="longitude", y="latitude", alpha=0.4,   
             s=housing["population"]/100, label="population", figsize=(10,7),  
             c="median_house_value", cmap=plt.get_cmap("jet"), colorbar=True, marker='s',)
plt.legend()
```




    <matplotlib.legend.Legend at 0x1e0e668a340>




​    
![png](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210181449931.png)
​    


## 使用corr()函数计算数据的相关性


```python
corr_matrix = housing.corr()
```

### 每个属性与房价中位数的相关性分别是多少:


```python
corr_matrix["median_house_value"].sort_values(ascending=False)
```




    median_house_value    1.000000
    median_income         0.687151
    total_rooms           0.135140
    housing_median_age    0.114146
    households            0.064590
    total_bedrooms        0.047781
    population           -0.026882
    longitude            -0.047466
    latitude             -0.142673
    Name: median_house_value, dtype: float64



### scatter_matrix函数 展示相关性


```python
# 由于scatter_matrix函数会测绘出每个属性相对于其他属性相关性的值，所以此处只展示几个相关性比较强的
from pandas.plotting import scatter_matrix
attributes = ["median_house_value", "median_income", "total_rooms",
 "housing_median_age"]
scatter_matrix(housing[attributes], figsize=(12, 8),marker='s',alpha=0.1)
```




    array([[<AxesSubplot:xlabel='median_house_value', ylabel='median_house_value'>,
            <AxesSubplot:xlabel='median_income', ylabel='median_house_value'>,
            <AxesSubplot:xlabel='total_rooms', ylabel='median_house_value'>,
            <AxesSubplot:xlabel='housing_median_age', ylabel='median_house_value'>],
           [<AxesSubplot:xlabel='median_house_value', ylabel='median_income'>,
            <AxesSubplot:xlabel='median_income', ylabel='median_income'>,
            <AxesSubplot:xlabel='total_rooms', ylabel='median_income'>,
            <AxesSubplot:xlabel='housing_median_age', ylabel='median_income'>],
           [<AxesSubplot:xlabel='median_house_value', ylabel='total_rooms'>,
            <AxesSubplot:xlabel='median_income', ylabel='total_rooms'>,
            <AxesSubplot:xlabel='total_rooms', ylabel='total_rooms'>,
            <AxesSubplot:xlabel='housing_median_age', ylabel='total_rooms'>],
           [<AxesSubplot:xlabel='median_house_value', ylabel='housing_median_age'>,
            <AxesSubplot:xlabel='median_income', ylabel='housing_median_age'>,
            <AxesSubplot:xlabel='total_rooms', ylabel='housing_median_age'>,
            <AxesSubplot:xlabel='housing_median_age', ylabel='housing_median_age'>]],
          dtype=object)




​    
![png](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210181449933.png)
​    


### 放大收入中位数与房价相关性的图


```python
housing.plot(kind="scatter", x="median_income",y="median_house_value",
             alpha=0.1)
```




    <AxesSubplot:xlabel='median_income', ylabel='median_house_value'>




​    
![png](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210181449934.png)
​    

## 实验不同属性的组合

通过不同属性相乘相除组成新的属性，再计算新的属性与标签之间的相关性


```python
# 单看“卧室总数”这个属性本身也没什么意义,你可能想拿它和“房间总数”来对比,
# 或者拿来同“每个家庭的人口数”这个属性组合似乎也挺有意思
housing["rooms_per_household"] = housing["total_rooms"]/housing["households"]
housing["bedrooms_per_room"] = housing["total_bedrooms"]/housing["total_rooms"]
housing["population_per_household"]=housing["population"]/housing["households"]
```

### 观察相关矩阵


```python
corr_matrix = housing.corr()
corr_matrix["median_house_value"].sort_values(ascending=False)
# 新的组合属性bedrooms_per_room较之“房间总数”或“卧室总数”与房价中位数的相关性都要高得多。
```




    median_house_value          1.000000
    median_income               0.687151
    rooms_per_household         0.146255
    total_rooms                 0.135140
    housing_median_age          0.114146
    households                  0.064590
    total_bedrooms              0.047781
    population_per_household   -0.021991
    population                 -0.026882
    longitude                  -0.047466
    latitude                   -0.142673
    bedrooms_per_room          -0.259952
    Name: median_house_value, dtype: float64



# 数据准备-清洗数据



## 回到一个干净的训练集


```python
#需要注意drop()会创建一个数据副本,但是不影响strat_train_set)
housing = strat_train_set.drop("median_house_value", axis=1)
housing_labels = strat_train_set["median_house_value"].copy()  #标签（房价中位数）
print(housing_labels.info(),'\n\n')
print(housing_labels.head(),'\n\n')
# housing["median_house_value"]=housing_labels
housing.head()
```

    <class 'pandas.core.series.Series'>
    Int64Index: 16512 entries, 12655 to 19773
    Series name: median_house_value
    Non-Null Count  Dtype  
    --------------  -----  
    16512 non-null  float64
    dtypes: float64(1)
    memory usage: 258.0 KB
    None 


​    
​    12655     72100.0
​    15502    279600.0
​    2908      82700.0
​    14053    112500.0
​    20496    238300.0
​    Name: median_house_value, dtype: float64 


​    
​    




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
      <th>longitude</th>
      <th>latitude</th>
      <th>housing_median_age</th>
      <th>total_rooms</th>
      <th>total_bedrooms</th>
      <th>population</th>
      <th>households</th>
      <th>median_income</th>
      <th>ocean_proximity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>12655</th>
      <td>-121.46</td>
      <td>38.52</td>
      <td>29.0</td>
      <td>3873.0</td>
      <td>797.0</td>
      <td>2237.0</td>
      <td>706.0</td>
      <td>2.1736</td>
      <td>INLAND</td>
    </tr>
    <tr>
      <th>15502</th>
      <td>-117.23</td>
      <td>33.09</td>
      <td>7.0</td>
      <td>5320.0</td>
      <td>855.0</td>
      <td>2015.0</td>
      <td>768.0</td>
      <td>6.3373</td>
      <td>NEAR OCEAN</td>
    </tr>
    <tr>
      <th>2908</th>
      <td>-119.04</td>
      <td>35.37</td>
      <td>44.0</td>
      <td>1618.0</td>
      <td>310.0</td>
      <td>667.0</td>
      <td>300.0</td>
      <td>2.8750</td>
      <td>INLAND</td>
    </tr>
    <tr>
      <th>14053</th>
      <td>-117.13</td>
      <td>32.75</td>
      <td>24.0</td>
      <td>1877.0</td>
      <td>519.0</td>
      <td>898.0</td>
      <td>483.0</td>
      <td>2.2264</td>
      <td>NEAR OCEAN</td>
    </tr>
    <tr>
      <th>20496</th>
      <td>-118.70</td>
      <td>34.28</td>
      <td>27.0</td>
      <td>3536.0</td>
      <td>646.0</td>
      <td>1837.0</td>
      <td>580.0</td>
      <td>4.4964</td>
      <td>&lt;1H OCEAN</td>
    </tr>
  </tbody>
</table>
</div>

## 处理数字型数据

### 处理缺失值的3种方法


```python
# housing.dropna(subset=["total_bedrooms"])    # option 1 
# housing.drop("total_bedrooms", axis=1)       # option 2

median = housing["total_bedrooms"].median()  # option 3
housing["total_bedrooms"].fillna(median, inplace=True)
# option 1： 去掉含有缺失值的样本（行）
# option 2：将含有缺失值的列（特征向量）去掉
# option 3：将缺失值用某些值填充（0，平均值，中值等）
```

### 选择第三种


```python
from sklearn.impute import SimpleImputer
imputer = SimpleImputer(strategy="median") #指明使用中位数来填充缺失值
imputer
```




    SimpleImputer(strategy='median')



#### 新建一个没有文本属性的df


```python
#由于中位数值只能在数值属性上计算,所以我们需要创建一个没有文本属性ocean_proximity的数据副本:
housing_num = housing.drop("ocean_proximity", axis=1)
housing_num
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
      <th>longitude</th>
      <th>latitude</th>
      <th>housing_median_age</th>
      <th>total_rooms</th>
      <th>total_bedrooms</th>
      <th>population</th>
      <th>households</th>
      <th>median_income</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>12655</th>
      <td>-121.46</td>
      <td>38.52</td>
      <td>29.0</td>
      <td>3873.0</td>
      <td>797.0</td>
      <td>2237.0</td>
      <td>706.0</td>
      <td>2.1736</td>
    </tr>
    <tr>
      <th>15502</th>
      <td>-117.23</td>
      <td>33.09</td>
      <td>7.0</td>
      <td>5320.0</td>
      <td>855.0</td>
      <td>2015.0</td>
      <td>768.0</td>
      <td>6.3373</td>
    </tr>
    <tr>
      <th>2908</th>
      <td>-119.04</td>
      <td>35.37</td>
      <td>44.0</td>
      <td>1618.0</td>
      <td>310.0</td>
      <td>667.0</td>
      <td>300.0</td>
      <td>2.8750</td>
    </tr>
    <tr>
      <th>14053</th>
      <td>-117.13</td>
      <td>32.75</td>
      <td>24.0</td>
      <td>1877.0</td>
      <td>519.0</td>
      <td>898.0</td>
      <td>483.0</td>
      <td>2.2264</td>
    </tr>
    <tr>
      <th>20496</th>
      <td>-118.70</td>
      <td>34.28</td>
      <td>27.0</td>
      <td>3536.0</td>
      <td>646.0</td>
      <td>1837.0</td>
      <td>580.0</td>
      <td>4.4964</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>15174</th>
      <td>-117.07</td>
      <td>33.03</td>
      <td>14.0</td>
      <td>6665.0</td>
      <td>1231.0</td>
      <td>2026.0</td>
      <td>1001.0</td>
      <td>5.0900</td>
    </tr>
    <tr>
      <th>12661</th>
      <td>-121.42</td>
      <td>38.51</td>
      <td>15.0</td>
      <td>7901.0</td>
      <td>1422.0</td>
      <td>4769.0</td>
      <td>1418.0</td>
      <td>2.8139</td>
    </tr>
    <tr>
      <th>19263</th>
      <td>-122.72</td>
      <td>38.44</td>
      <td>48.0</td>
      <td>707.0</td>
      <td>166.0</td>
      <td>458.0</td>
      <td>172.0</td>
      <td>3.1797</td>
    </tr>
    <tr>
      <th>19140</th>
      <td>-122.70</td>
      <td>38.31</td>
      <td>14.0</td>
      <td>3155.0</td>
      <td>580.0</td>
      <td>1208.0</td>
      <td>501.0</td>
      <td>4.1964</td>
    </tr>
    <tr>
      <th>19773</th>
      <td>-122.14</td>
      <td>39.97</td>
      <td>27.0</td>
      <td>1079.0</td>
      <td>222.0</td>
      <td>625.0</td>
      <td>197.0</td>
      <td>3.1319</td>
    </tr>
  </tbody>
</table>
<p>16512 rows × 8 columns</p>
</div>



#### fit()将imputer实例适配到训练数据


```python
imputer.fit(housing_num)
```




    SimpleImputer(strategy='median')




```python
# 这里imputer仅仅只是计算了每个属性的中位数值,并将结果存储在其实例变量statistics_中
print(imputer.statistics_,'\n\n')
print(housing_num.median().values)
```

    [-118.51      34.26      29.      2119.       433.      1164.
      408.         3.54155] 


​    
​    [-118.51      34.26      29.      2119.       433.      1164.
​      408.         3.54155]


#### 填充null数据


```python
# 现在,你可以使用这个“训练有素”的imputer将缺失值替换成中位数值从而完成训练集转换
X = imputer.transform(housing_num)
print(X)    #将每一行的数据以数组的形式打印出来
print(housing_num)
```

    [[-1.2146e+02  3.8520e+01  2.9000e+01 ...  2.2370e+03  7.0600e+02
       2.1736e+00]
     [-1.1723e+02  3.3090e+01  7.0000e+00 ...  2.0150e+03  7.6800e+02
       6.3373e+00]
     [-1.1904e+02  3.5370e+01  4.4000e+01 ...  6.6700e+02  3.0000e+02
       2.8750e+00]
     ...
     [-1.2272e+02  3.8440e+01  4.8000e+01 ...  4.5800e+02  1.7200e+02
       3.1797e+00]
     [-1.2270e+02  3.8310e+01  1.4000e+01 ...  1.2080e+03  5.0100e+02
       4.1964e+00]
     [-1.2214e+02  3.9970e+01  2.7000e+01 ...  6.2500e+02  1.9700e+02
       3.1319e+00]]
           longitude  latitude  housing_median_age  total_rooms  total_bedrooms  \
    12655    -121.46     38.52                29.0       3873.0           797.0   
    15502    -117.23     33.09                 7.0       5320.0           855.0   
    2908     -119.04     35.37                44.0       1618.0           310.0   
    14053    -117.13     32.75                24.0       1877.0           519.0   
    20496    -118.70     34.28                27.0       3536.0           646.0   
    ...          ...       ...                 ...          ...             ...   
    15174    -117.07     33.03                14.0       6665.0          1231.0   
    12661    -121.42     38.51                15.0       7901.0          1422.0   
    19263    -122.72     38.44                48.0        707.0           166.0   
    19140    -122.70     38.31                14.0       3155.0           580.0   
    19773    -122.14     39.97                27.0       1079.0           222.0   
    
           population  households  median_income  
    12655      2237.0       706.0         2.1736  
    15502      2015.0       768.0         6.3373  
    2908        667.0       300.0         2.8750  
    14053       898.0       483.0         2.2264  
    20496      1837.0       580.0         4.4964  
    ...           ...         ...            ...  
    15174      2026.0      1001.0         5.0900  
    12661      4769.0      1418.0         2.8139  
    19263       458.0       172.0         3.1797  
    19140      1208.0       501.0         4.1964  
    19773       625.0       197.0         3.1319  
    
    [16512 rows x 8 columns]


#### 填充后数组数据转换成df


```python
#结果是一个包含转换后特征的NumPy数组。如果你想将它放回pandas DataFrame
housing_tr = pd.DataFrame(X, columns=housing_num.columns,                       
                          index=housing_num.index)

print(housing_tr.info(),'\n\n')
print(housing_num.columns)
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 16512 entries, 12655 to 19773
    Data columns (total 8 columns):
     #   Column              Non-Null Count  Dtype  
    ---  ------              --------------  -----  
     0   longitude           16512 non-null  float64
     1   latitude            16512 non-null  float64
     2   housing_median_age  16512 non-null  float64
     3   total_rooms         16512 non-null  float64
     4   total_bedrooms      16512 non-null  float64
     5   population          16512 non-null  float64
     6   households          16512 non-null  float64
     7   median_income       16512 non-null  float64
    dtypes: float64(8)
    memory usage: 1.1 MB
    None 


​    
​    Index(['longitude', 'latitude', 'housing_median_age', 'total_rooms',
​           'total_bedrooms', 'population', 'households', 'median_income'],
​          dtype='object')


## 处理文本和分类属性


```python
housing_cat = housing[["ocean_proximity"]]
housing_cat.head(10)
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
      <th>ocean_proximity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>12655</th>
      <td>INLAND</td>
    </tr>
    <tr>
      <th>15502</th>
      <td>NEAR OCEAN</td>
    </tr>
    <tr>
      <th>2908</th>
      <td>INLAND</td>
    </tr>
    <tr>
      <th>14053</th>
      <td>NEAR OCEAN</td>
    </tr>
    <tr>
      <th>20496</th>
      <td>&lt;1H OCEAN</td>
    </tr>
    <tr>
      <th>1481</th>
      <td>NEAR BAY</td>
    </tr>
    <tr>
      <th>18125</th>
      <td>&lt;1H OCEAN</td>
    </tr>
    <tr>
      <th>5830</th>
      <td>&lt;1H OCEAN</td>
    </tr>
    <tr>
      <th>17989</th>
      <td>&lt;1H OCEAN</td>
    </tr>
    <tr>
      <th>4861</th>
      <td>&lt;1H OCEAN</td>
    </tr>
  </tbody>
</table>
</div>



### 方法一

#### 将这些类别从文本转到数字


```python
from sklearn.preprocessing import OrdinalEncoder  #我们将这些类别从文本转到数字
ordinal_encoder = OrdinalEncoder() 
housing_cat_encoded = ordinal_encoder.fit_transform(housing_cat) 
housing_cat_encoded[:10] #打印前十行

# numpy[行：行：]
# numpy[：；列：列]
# housing_cat_encoded
# housing_cat.info()
```




    array([[1.],
           [4.],
           [1.],
           [4.],
           [0.],
           [3.],
           [0.],
           [0.],
           [0.],
           [0.]])



#### Categories_实例变量获取类别列表


```python
ordinal_encoder.categories_
```




    [array(['<1H OCEAN', 'INLAND', 'ISLAND', 'NEAR BAY', 'NEAR OCEAN'],
           dtype=object)]



### 方法二

#### 使用SciPy稀疏矩阵擦除距离噪音


```python
# 这种表征方式产生的一个问题是,机器学习算法会认为两个相近的值比两个离得较远的值更为相似一些。
from sklearn.preprocessing import OneHotEncoder 
cat_encoder = OneHotEncoder() 
housing_cat_1hot = cat_encoder.fit_transform(housing_cat) 
housing_cat_1hot.toarray()
#一共分为了种向量，每种向量代表了一种分类，并且分类之间的距离都是相等的
```




    array([[0., 1., 0., 0., 0.],
           [0., 0., 0., 0., 1.],
           [0., 1., 0., 0., 0.],
           ...,
           [1., 0., 0., 0., 0.],
           [1., 0., 0., 0., 0.],
           [0., 1., 0., 0., 0.]])




```python
cat_encoder.categories_
```




    [array(['<1H OCEAN', 'INLAND', 'ISLAND', 'NEAR BAY', 'NEAR OCEAN'],
           dtype=object)]



## 自定义转换器

现在的理解是，定义一个函数，设置几个属性参数，根据设置的超参数的值，来判断，使用哪一种方式对数据进行处理，并将得到的新的属性列加入到数组中


```python
from sklearn.base import BaseEstimator, TransformerMixin

rooms_ix, bedrooms_ix, population_ix, households_ix = 3, 4, 5, 6  

class CombinedAttributesAdder(BaseEstimator, TransformerMixin):     
    def __init__(self, add_bedrooms_per_room = True): # 类似于构造器，这里传入的是一个布尔类型的数值        
        self.add_bedrooms_per_room = add_bedrooms_per_room     
    
    def fit(self, X, y=None):         
        return self  # nothing else to do     
    
    def transform(self, X):         
        rooms_per_household = X[:, rooms_ix] / X[:, households_ix]         
        population_per_household = X[:, population_ix] / X[:, households_ix]         
        
        if self.add_bedrooms_per_room:             
            bedrooms_per_room = X[:, bedrooms_ix] / X[:, rooms_ix]             
            return np.c_[X, rooms_per_household, population_per_household,                          
                         bedrooms_per_room]         
        else:             
            return np.c_[X, rooms_per_household, population_per_household]  
        
attr_adder = CombinedAttributesAdder(add_bedrooms_per_room=False) 
housing_extra_attribs = attr_adder.transform(housing.values)
```

## 定义转换流水线

目前的理解就是，流水线上都是一些已经封装好的函数，然后根据在流水线上的顺序运行，对数据进行处理

### 初代流水线


```python
from sklearn.pipeline import Pipeline 
from sklearn.preprocessing import StandardScaler  

num_pipeline = Pipeline([         
    ('imputer', SimpleImputer(strategy="median")),      # 用中位数填充缺失值   
    ('attribs_adder', CombinedAttributesAdder()),         #调用自定义转换器
    ('std_scaler', StandardScaler()),     ])  #对数据进行特征缩放       

housing_num_tr = num_pipeline.fit_transform(housing_num)
```

### ColumnTransformer处理列

1. 提取不同转换器所需要的列名


```python
num_attribs = list(housing_num) #数值列名称列表和类别列名称列表
cat_attribs = ["ocean_proximity"]

print(num_attribs)
print(cat_attribs)
```

    ['longitude', 'latitude', 'housing_median_age', 'total_rooms', 'total_bedrooms', 'population', 'households', 'median_income']
    
    ['ocean_proximity']

 


2. 将对应的列名分别划分到ColumnTransformer实例对象中


```python
from sklearn.compose import ColumnTransformer  

num_attribs = list(housing_num) #数值列名称列表和类别列名称列表
cat_attribs = ["ocean_proximity"]

full_pipeline = ColumnTransformer([         
    ("num", num_pipeline, num_attribs),         
    ("cat", OneHotEncoder(), cat_attribs),     ])  

housing_prepared = full_pipeline.fit_transform(housing)
```


```python
housing_labels
```




    12655     72100.0
    15502    279600.0
    2908      82700.0
    14053    112500.0
    20496    238300.0
               ...   
    15174    268500.0
    12661     90400.0
    19263    140400.0
    19140    258100.0
    19773     62700.0
    Name: median_house_value, Length: 16512, dtype: float64



# 选择和训练模型

## 选择线性回归模型


```python
from sklearn.linear_model import LinearRegression  
lin_reg = LinearRegression() 
lin_reg.fit(housing_prepared, housing_labels)
#这里传入的是，清洗过后的训练集信息，以及要预测的真实结果的信息
#房子的信息表  房子的平均房价
#此时是已经得到线性回归的模型了
```




    LinearRegression()




```python
#从数据总集中选择部分数据进行检验
some_data = housing.iloc[:5]   #获取测使用的原始数据
some_labels = housing_labels.iloc[:5]  #获取改组数据对应的原始标签

some_data_prepared = full_pipeline.transform(some_data) #对数据进行清洗

#使用刚训练好的模型，将部分房子信息传入该模型，并预测平均房价
print("Predictions:", lin_reg.predict(some_data_prepared))

#将真正的平均房价打印出来
print("Labels:", list(some_labels))  
```

    Predictions: [ 85657.90192014 305492.60737488 152056.46122456 186095.70946094
     244550.67966089]
    Labels: [72100.0, 279600.0, 82700.0, 112500.0, 238300.0]



```python
# 计算标签值与预测值的均方误差

from sklearn.metrics import mean_squared_error 

housing_predictions = lin_reg.predict(housing_prepared) #计算预测值
lin_mse = mean_squared_error(housing_labels, housing_predictions) #计算标签与预测值的方差
lin_rmse = np.sqrt(lin_mse) #对求得的方差开平方
lin_rmse #计算得出结果
```




    68627.87390018743



## DecisionTreeRegressor 决策树-模型


```python
#决策树模型
from sklearn.tree import DecisionTreeRegressor  
tree_reg = DecisionTreeRegressor() 
tree_reg.fit(housing_prepared, housing_labels)
```




    DecisionTreeRegressor()




```python
housing_predictions = tree_reg.predict(housing_prepared)
tree_mse = mean_squared_error(housing_labels, housing_predictions) 
tree_rmse = np.sqrt(tree_mse) 
tree_rmse   #下面的结果看起来模型有点过拟合了
```




    0.0



## K-折交叉验证模型适配度


```python
# 将原始数据分成k组，然后对原始数据进行k次训练
# 每次选取k-1组数据作为训练界集，然后用剩下的那一组数据作为测试集
# 直到每一组数据都参与过测试集
```

### 评估决策树模型


```python
#多重交叉检验
from sklearn.model_selection import cross_val_score 
scores = cross_val_score(tree_reg, housing_prepared, housing_labels,                          
                         scoring="neg_mean_squared_error", cv=10) 
#cv = 10 表示k=10，进行十次交叉验证
tree_rmse_scores = np.sqrt(-scores)
```


```python
print(scores)
print(tree_rmse_scores)
#因为进行了十次实验，所以结果有十个方差，仔细观察下面的数据会发现，其实比线性回归还要差一些
```

    [-5.22529590e+09 -5.03397664e+09 -4.67038492e+09 -5.05155766e+09
     -4.83730440e+09 -5.92500413e+09 -5.07351787e+09 -5.20201117e+09
     -4.75379664e+09 -5.30317828e+09]
    [72286.20822504 70950.52250254 68340.21451777 71074.31085591
     69550.73255507 76974.04840576 71228.63099946 72124.96911861
     68947.7819725  72822.92416863]


#### 模型的评分


```python
#交叉验证模型
def display_scores(scores):
    print("Scores:", scores) # 打印原始十次的平均误差
    print("Mean:", scores.mean()) # 打印方差列表中位数
    print("Standard deviation:", scores.std()) # 打印十次原始标准差
    
display_scores(tree_rmse_scores)
```

    Scores: [72286.20822504 70950.52250254 68340.21451777 71074.31085591
     69550.73255507 76974.04840576 71228.63099946 72124.96911861
     68947.7819725  72822.92416863]
    Mean: 71430.03433213099
    Standard deviation: 2313.6459270520104


### 评估回归模型，并打分


```python
#回归模型
lin_scores = cross_val_score(lin_reg, housing_prepared, housing_labels,
                             scoring="neg_mean_squared_error", cv=10) 

lin_rmse_scores = np.sqrt(-lin_scores) 
display_scores(lin_rmse_scores)
```

    Scores: [71762.76364394 64114.99166359 67771.17124356 68635.19072082
     66846.14089488 72528.03725385 73997.08050233 68802.33629334
     66443.28836884 70139.79923956]
    Mean: 69104.07998247063
    Standard deviation: 2880.3282098180634


## RandomForestRegressor 随机森林模型


```python
from sklearn.ensemble import RandomForestRegressor 
forest_reg = RandomForestRegressor() 
forest_reg.fit(housing_prepared, housing_labels)
housing_predictions = forest_reg.predict(housing_prepared)
forest_mse = mean_squared_error(housing_labels, housing_predictions) #计算标签与预测值的方差
forest_rmse = np.sqrt(lin_mse) #对求得的方差开平方
forest_rmse #计算得出结果
```




    68627.87390018743

### 评分


```python
# 对随机森林模型评估并打分
forest_scores = cross_val_score(forest_reg, housing_prepared, housing_labels,
                             scoring="neg_mean_squared_error", cv=10) 

forest_rmse_scores = np.sqrt(-forest_scores) 

display_scores(forest_rmse_scores)
```

    Scores: [51438.56192311 48850.25170124 46935.71492762 51874.53732343
     47761.3870821  51949.25233623 52293.64505688 49945.05780461
     48727.52154891 54046.36660109]
    Mean: 50382.22963052139
    Standard deviation: 2165.6685496987893



```python
#将计算结果保存起来
import joblib  
joblib.dump(forest_rmse_scores, "my_model.pkl") 
# and later... 
my_model_loaded = joblib.load("my_model.pkl")
```


```python
my_model_loaded
```




    array([51438.56192311, 48850.25170124, 46935.71492762, 51874.53732343,
           47761.3870821 , 51949.25233623, 52293.64505688, 49945.05780461,
           48727.52154891, 54046.36660109])



# 微调模型

## GridSearchCV 网格搜索


```python
from sklearn.model_selection import GridSearchCV  
param_grid = [     
    {'n_estimators': [3, 10, 30], 'max_features': [2, 4, 6, 8]},     
    {'bootstrap': [False], 'n_estimators': [3, 10], 'max_features': [2, 3, 4]},   
    ]  
forest_reg = RandomForestRegressor()  
grid_search = GridSearchCV(forest_reg, param_grid, cv=5,                            
                           scoring='neg_mean_squared_error',                            
                           return_train_score=True)  
grid_search.fit(housing_prepared, housing_labels)
```




    GridSearchCV(cv=5, estimator=RandomForestRegressor(),
                 param_grid=[{'max_features': [2, 4, 6, 8],
                              'n_estimators': [3, 10, 30]},
                             {'bootstrap': [False], 'max_features': [2, 3, 4],
                              'n_estimators': [3, 10]}],
                 return_train_score=True, scoring='neg_mean_squared_error')




```python
grid_search.best_params_  # 上面一共训练出了18组得分，这里打印出训练得分最好的一组
```




    {'max_features': 6, 'n_estimators': 30}




```python
#得到最好的估算器
grid_search.best_estimator_
```




    RandomForestRegressor(max_features=6, n_estimators=30)




```python
#查看18组各自的得分
cvres = grid_search.cv_results_ 

for mean_score, params in zip(cvres["mean_test_score"], cvres["params"]): 
    print(np.sqrt(-mean_score), params)
```

    63795.81964406407 {'max_features': 2, 'n_estimators': 3}
    55336.97050306765 {'max_features': 2, 'n_estimators': 10}
    52442.53458342377 {'max_features': 2, 'n_estimators': 30}
    60939.04896445967 {'max_features': 4, 'n_estimators': 3}
    52652.943677104166 {'max_features': 4, 'n_estimators': 10}
    50291.53243142402 {'max_features': 4, 'n_estimators': 30}
    59213.931890329164 {'max_features': 6, 'n_estimators': 3}
    52145.57753016071 {'max_features': 6, 'n_estimators': 10}
    50136.41742763945 {'max_features': 6, 'n_estimators': 30}
    58785.83002875949 {'max_features': 8, 'n_estimators': 3}
    52482.71422538591 {'max_features': 8, 'n_estimators': 10}
    50178.950375902976 {'max_features': 8, 'n_estimators': 30}
    60634.48177417556 {'bootstrap': False, 'max_features': 2, 'n_estimators': 3}
    54464.86442132568 {'bootstrap': False, 'max_features': 2, 'n_estimators': 10}
    60218.15304206291 {'bootstrap': False, 'max_features': 3, 'n_estimators': 3}
    52587.36558391441 {'bootstrap': False, 'max_features': 3, 'n_estimators': 10}
    58681.51868775598 {'bootstrap': False, 'max_features': 4, 'n_estimators': 3}
    51369.92634646682 {'bootstrap': False, 'max_features': 4, 'n_estimators': 10}


## 分析每列的重要性并打分


```python
# RandomForestRegressor可以指出每个属性的相对重要程度
feature_importances = grid_search.best_estimator_.feature_importances_ 
feature_importances
```




    array([7.54594962e-02, 7.22149898e-02, 4.21707677e-02, 1.67962508e-02,
           1.69875315e-02, 1.66898697e-02, 1.58618362e-02, 3.24701609e-01,
           4.74660479e-02, 1.09747426e-01, 9.61019780e-02, 1.04827380e-02,
           1.47012256e-01, 8.65175300e-05, 3.81623401e-03, 4.40445109e-03])




```python
#将这些重要性分数显示在对应的属性名称旁边
extra_attribs = ["rooms_per_hhold", "pop_per_hhold", "bedrooms_per_room"] 
cat_encoder = full_pipeline.named_transformers_["cat"] 
cat_one_hot_attribs = list(cat_encoder.categories_[0]) 
attributes = num_attribs + extra_attribs + cat_one_hot_attribs 
sorted(zip(feature_importances, attributes), reverse=True)
```




    [(0.3247016091226939, 'median_income'),
     (0.1470122562239448, 'INLAND'),
     (0.10974742617981405, 'pop_per_hhold'),
     (0.0961019779997854, 'bedrooms_per_room'),
     (0.0754594962213714, 'longitude'),
     (0.0722149898255641, 'latitude'),
     (0.04746604786977869, 'rooms_per_hhold'),
     (0.04217076768276524, 'housing_median_age'),
     (0.016987531541087827, 'total_bedrooms'),
     (0.01679625080333917, 'total_rooms'),
     (0.01668986965391197, 'population'),
     (0.01586183620593413, 'households'),
     (0.010482738035163598, '<1H OCEAN'),
     (0.004404451094549737, 'NEAR OCEAN'),
     (0.003816234010275334, 'NEAR BAY'),
     (8.651753002065567e-05, 'ISLAND')]



# 使用测试集来进行评估


```python
final_model = grid_search.best_estimator_   #模型选择的是之前王哥选择的最好的模型
X_test = strat_test_set.drop("median_house_value", axis=1) #去掉测试集的标签列
y_test = strat_test_set["median_house_value"].copy()  #获取标签列
X_test_prepared = full_pipeline.transform(X_test) #清洗测试集的原始数据
final_predictions = final_model.predict(X_test_prepared)  #使用最优的模型对测试集进行预测

final_mse = mean_squared_error(y_test, final_predictions) #评估测试集于标签之间的差距
final_rmse = np.sqrt(final_mse)   # => evaluates to 47,730.2
```


```python
final_rmse
```




    47958.70852671495

