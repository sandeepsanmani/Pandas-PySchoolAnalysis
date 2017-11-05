

```python
#Import dependencies
import pandas as pd
import csv
import numpy as np
```


```python
#Reading the csv file
file_path = "raw_data/schools_complete.csv"
schools_df = pd.read_csv(file_path)
schools_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>name</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Reading the csv file
file_path = "raw_data/students_complete.csv"
students_df = pd.read_csv(file_path)
students_df.head()

df = students_df.groupby(["school"])['reading_score'].sum()
df
```




    school
    Bailey High School       403225
    Cabrera High School      156027
    Figueroa High School     239335
    Ford High School         221164
    Griffin High School      123043
    Hernandez High School    375131
    Holden High School        35789
    Huang High School        236810
    Johnson High School      385481
    Pena High School          80851
    Rodriguez High School    322898
    Shelton High School      147441
    Thomas High School       137093
    Wilson High School       191748
    Wright High School       151119
    Name: reading_score, dtype: int64




```python
renamed_schools_df = schools_df.rename(columns={"name":"school", "size": "Total Students", "budget": "Total Budget"})
renamed_schools_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>school</th>
      <th>type</th>
      <th>Total Students</th>
      <th>Total Budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
    </tr>
  </tbody>
</table>
</div>




```python
combined_data = pd.merge(students_df,renamed_schools_df, on = "school")
new_dataset = combined_data.drop(["Student ID", "gender","grade","name"], axis =1)
new_dataset.head()
grouped_data = new_dataset.groupby("school")

grouped_data
```




    <pandas.core.groupby.DataFrameGroupBy object at 0x000002524E19E908>




```python
#math_scores[(math_scores["math_score"] > 65)]
#grouped_data.filter(lambda x: x["math_score"] > 65)

```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-273-ad5dd95bae28> in <module>()
          1 #math_scores[(math_scores["math_score"] > 65)]
    ----> 2 grouped_data.filter(lambda x: x["math_score"] > 65)
    

    C:\tools\Anaconda3\lib\site-packages\pandas\core\groupby.py in filter(self, func, dropna, *args, **kwargs)
       3958                 raise TypeError("filter function returned a %s, "
       3959                                 "but expected a scalar bool" %
    -> 3960                                 type(res).__name__)
       3961 
       3962         return self._apply_filter(indices, dropna)
    

    TypeError: filter function returned a Series, but expected a scalar bool



```python
#Total # of schools
total_schools = new_dataset["School ID"].value_counts()
no_of_schools = total_schools.count()
no_of_schools
```




    15




```python
#Total # of students
total_students = students_df["name"].count()
total_students
```




    39170




```python
#Total school budget
total_budget = schools_df["budget"].sum()
total_budget
```




    24649428




```python
#Average math score
avg_math_score = round(students_df["math_score"].mean(),2)
avg_math_score
```




    78.99




```python
#Average reading score
avg_reading_score = round(students_df["reading_score"].mean(),2)
avg_reading_score
```




    81.88




```python
#% passing math
total_students_passing_math = students_df[students_df["math_score"] >= 65].count()["math_score"]
total_students_passing_math
percentage_passing_math = (total_students_passing_math/total_students)*100
round_percentage_passing_math = round(percentage_passing_math,2)
round_percentage_passing_math
```




    84.730000000000004




```python
# % passing reading
total_students_passing_read = students_df[students_df["reading_score"]>=65].count()["reading_score"]
total_students_passing_read
percentage_passing_reading = round((total_students_passing_read/total_students)*100,2)
percentage_passing_reading
```




    96.200000000000003




```python
overall_pass = pd.DataFrame([percentage_passing_math,percentage_passing_reading])
overall_pass
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>84.728108</td>
    </tr>
    <tr>
      <th>1</th>
      <td>96.200000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Overall passing rate
overall_passing_rate = round(overall_pass.mean()[0],2)
overall_passing_rate
```




    90.459999999999994




```python

student_summary = pd.DataFrame ([{"Total Schools":total_schools,"Total Students": total_students, "Total School Budget" : total_budget, "Average Math Score": avg_math_score, "Average Reading Score": avg_reading_score,"Percentage Passing Math": percentage_passing_math, "Percentage Passing Reading": percentage_passing_reading, "Overall Passing Rate": overall_passing_rate}])
student_summary
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Overall Passing Rate</th>
      <th>Percentage Passing Math</th>
      <th>Percentage Passing Reading</th>
      <th>Total School Budget</th>
      <th>Total Schools</th>
      <th>Total Students</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>78.99</td>
      <td>81.88</td>
      <td>90.46</td>
      <td>84.728108</td>
      <td>96.2</td>
      <td>24649428</td>
      <td>7     4976
12    4761
3     4635
11    3999
1 ...</td>
      <td>39170</td>
    </tr>
  </tbody>
</table>
</div>




```python
math_scores = new_dataset.drop(["reading_score","School ID","type","Total Students","Total Budget"],axis =1)
math_scores
math_passed = math_scores[(math_scores["math_score"] > 65)]
schoolwise_math_passed = math_passed.groupby("school") 
total_passed = pd.DataFrame(schoolwise_math_passed.count())
total_passed

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>math_score</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>3762</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>1858</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>2208</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>2087</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>1468</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>3498</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>427</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>2196</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>3589</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>962</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>3021</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>1761</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>1635</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>2283</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>1800</td>
    </tr>
  </tbody>
</table>
</div>




```python
reading_scores = new_dataset.drop(["math_score","School ID","type","Total Students","Total Budget"],axis =1)
reading_scores
reading_passed = reading_scores[(reading_scores["reading_score"] > 65)]
schoolwise_reading_passed = reading_passed.groupby("school") 
total_reading_passed = pd.DataFrame(schoolwise_reading_passed.count())
total_reading_passed
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>reading_score</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>4573</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>1858</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>2710</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>2487</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>1468</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>4239</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>427</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>2677</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>4383</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>962</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>3660</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>1761</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>1635</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>2283</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>1800</td>
    </tr>
  </tbody>
</table>
</div>




```python
total_passed["reading_score"] = total_reading_passed["reading_score"]
total_passed
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>math_score</th>
      <th>reading_score</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>3762</td>
      <td>4573</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>1858</td>
      <td>1858</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>2208</td>
      <td>2710</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>2087</td>
      <td>2487</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>1468</td>
      <td>1468</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>3498</td>
      <td>4239</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>427</td>
      <td>427</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>2196</td>
      <td>2677</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>3589</td>
      <td>4383</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>962</td>
      <td>962</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>3021</td>
      <td>3660</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>1761</td>
      <td>1761</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>1635</td>
      <td>1635</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>2283</td>
      <td>2283</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>1800</td>
      <td>1800</td>
    </tr>
  </tbody>
</table>
</div>




```python
grouped_data = new_dataset.groupby(["school"], as_index=False)
print(grouped_data)
a = pd.DataFrame(grouped_data.count())
a
```

    <pandas.core.groupby.DataFrameGroupBy object at 0x000002524F1019B0>
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>School ID</th>
      <th>type</th>
      <th>Total Students</th>
      <th>Total Budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bailey High School</td>
      <td>4976</td>
      <td>4976</td>
      <td>4976</td>
      <td>4976</td>
      <td>4976</td>
      <td>4976</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cabrera High School</td>
      <td>1858</td>
      <td>1858</td>
      <td>1858</td>
      <td>1858</td>
      <td>1858</td>
      <td>1858</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Figueroa High School</td>
      <td>2949</td>
      <td>2949</td>
      <td>2949</td>
      <td>2949</td>
      <td>2949</td>
      <td>2949</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ford High School</td>
      <td>2739</td>
      <td>2739</td>
      <td>2739</td>
      <td>2739</td>
      <td>2739</td>
      <td>2739</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>1468</td>
      <td>1468</td>
      <td>1468</td>
      <td>1468</td>
      <td>1468</td>
      <td>1468</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Hernandez High School</td>
      <td>4635</td>
      <td>4635</td>
      <td>4635</td>
      <td>4635</td>
      <td>4635</td>
      <td>4635</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Holden High School</td>
      <td>427</td>
      <td>427</td>
      <td>427</td>
      <td>427</td>
      <td>427</td>
      <td>427</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Huang High School</td>
      <td>2917</td>
      <td>2917</td>
      <td>2917</td>
      <td>2917</td>
      <td>2917</td>
      <td>2917</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Johnson High School</td>
      <td>4761</td>
      <td>4761</td>
      <td>4761</td>
      <td>4761</td>
      <td>4761</td>
      <td>4761</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>962</td>
      <td>962</td>
      <td>962</td>
      <td>962</td>
      <td>962</td>
      <td>962</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Rodriguez High School</td>
      <td>3999</td>
      <td>3999</td>
      <td>3999</td>
      <td>3999</td>
      <td>3999</td>
      <td>3999</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Shelton High School</td>
      <td>1761</td>
      <td>1761</td>
      <td>1761</td>
      <td>1761</td>
      <td>1761</td>
      <td>1761</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Thomas High School</td>
      <td>1635</td>
      <td>1635</td>
      <td>1635</td>
      <td>1635</td>
      <td>1635</td>
      <td>1635</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Wilson High School</td>
      <td>2283</td>
      <td>2283</td>
      <td>2283</td>
      <td>2283</td>
      <td>2283</td>
      <td>2283</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Wright High School</td>
      <td>1800</td>
      <td>1800</td>
      <td>1800</td>
      <td>1800</td>
      <td>1800</td>
      <td>1800</td>
    </tr>
  </tbody>
</table>
</div>




```python
data = pd.merge(total_passed,a, on = "school",  how="outer")
data_df = pd.DataFrame(data)
data_df

```


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    C:\tools\Anaconda3\lib\site-packages\pandas\core\indexes\base.py in get_loc(self, key, method, tolerance)
       2441             try:
    -> 2442                 return self._engine.get_loc(key)
       2443             except KeyError:
    

    pandas\_libs\index.pyx in pandas._libs.index.IndexEngine.get_loc()
    

    pandas\_libs\index.pyx in pandas._libs.index.IndexEngine.get_loc()
    

    pandas\_libs\hashtable_class_helper.pxi in pandas._libs.hashtable.PyObjectHashTable.get_item()
    

    pandas\_libs\hashtable_class_helper.pxi in pandas._libs.hashtable.PyObjectHashTable.get_item()
    

    KeyError: 'school'

    
    During handling of the above exception, another exception occurred:
    

    KeyError                                  Traceback (most recent call last)

    <ipython-input-286-49e439b9bfd9> in <module>()
    ----> 1 data = pd.merge(total_passed,a, on = "school",  how="outer")
          2 data_df = pd.DataFrame(data)
          3 data_df
    

    C:\tools\Anaconda3\lib\site-packages\pandas\core\reshape\merge.py in merge(left, right, how, on, left_on, right_on, left_index, right_index, sort, suffixes, copy, indicator)
         51                          right_on=right_on, left_index=left_index,
         52                          right_index=right_index, sort=sort, suffixes=suffixes,
    ---> 53                          copy=copy, indicator=indicator)
         54     return op.get_result()
         55 
    

    C:\tools\Anaconda3\lib\site-packages\pandas\core\reshape\merge.py in __init__(self, left, right, how, on, left_on, right_on, axis, left_index, right_index, sort, suffixes, copy, indicator)
        556         (self.left_join_keys,
        557          self.right_join_keys,
    --> 558          self.join_names) = self._get_merge_keys()
        559 
        560         # validate the merge keys dtypes. We may need to coerce
    

    C:\tools\Anaconda3\lib\site-packages\pandas\core\reshape\merge.py in _get_merge_keys(self)
        821                         right_keys.append(rk)
        822                     if lk is not None:
    --> 823                         left_keys.append(left[lk]._values)
        824                         join_names.append(lk)
        825                     else:
    

    C:\tools\Anaconda3\lib\site-packages\pandas\core\frame.py in __getitem__(self, key)
       1962             return self._getitem_multilevel(key)
       1963         else:
    -> 1964             return self._getitem_column(key)
       1965 
       1966     def _getitem_column(self, key):
    

    C:\tools\Anaconda3\lib\site-packages\pandas\core\frame.py in _getitem_column(self, key)
       1969         # get column
       1970         if self.columns.is_unique:
    -> 1971             return self._get_item_cache(key)
       1972 
       1973         # duplicate columns & possible reduce dimensionality
    

    C:\tools\Anaconda3\lib\site-packages\pandas\core\generic.py in _get_item_cache(self, item)
       1643         res = cache.get(item)
       1644         if res is None:
    -> 1645             values = self._data.get(item)
       1646             res = self._box_item_values(item, values)
       1647             cache[item] = res
    

    C:\tools\Anaconda3\lib\site-packages\pandas\core\internals.py in get(self, item, fastpath)
       3588 
       3589             if not isnull(item):
    -> 3590                 loc = self.items.get_loc(item)
       3591             else:
       3592                 indexer = np.arange(len(self.items))[isnull(self.items)]
    

    C:\tools\Anaconda3\lib\site-packages\pandas\core\indexes\base.py in get_loc(self, key, method, tolerance)
       2442                 return self._engine.get_loc(key)
       2443             except KeyError:
    -> 2444                 return self._engine.get_loc(self._maybe_cast_indexer(key))
       2445 
       2446         indexer = self.get_indexer([key], method=method, tolerance=tolerance)
    

    pandas\_libs\index.pyx in pandas._libs.index.IndexEngine.get_loc()
    

    pandas\_libs\index.pyx in pandas._libs.index.IndexEngine.get_loc()
    

    pandas\_libs\hashtable_class_helper.pxi in pandas._libs.hashtable.PyObjectHashTable.get_item()
    

    pandas\_libs\hashtable_class_helper.pxi in pandas._libs.hashtable.PyObjectHashTable.get_item()
    

    KeyError: 'school'



```python
schoolwise_data["Average Math Score"] = round(schoolwise_data["math_score"]/schoolwise_data["Total Students"],2)
schoolwise_data["Average Reading Score"] = round(schoolwise_data["reading_score"]/schoolwise_data["Total Students"],2)
schoolwise_data
```


```python
students_grouped = students_df.groupby(by=['school','math_score',"reading_score"]).sum()
students_grouped.head()
#student_details = students_passing_math.apply(lambda g: g[g['math_score'] >= 65])["math_score"]

```
