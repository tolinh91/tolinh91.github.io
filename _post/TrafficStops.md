# Introduction
On a typical day in the United States, police officers make more than 50,000 traffic stops.
The Stanford Open Policing Project is motivated for gathering, analyzing, and releasing records from millions of traffic stops by law enforcement agencies across 31 States of USA. 
Their goal is to help researchers, journalists, and policymakers investigate and improve interactions between police and the public.

The below is my practice for analyzing the data of traffic stops in the state of Rhode Island.
The related data is available in this [link](https://openpolicing.stanford.edu/data/). Please choose RI (State Patrol) to download.

# Examining the data set

```python
"""
Read the dataset into pandas.
Examine data using .isnull() and count the number of missing value by .isnull().sum().
Drop NA values in columns 'county_name' and 'state' using .dropna().
Args:
  ri(DataFrame): the csv file to read

Returns:
  DataFrame
"""
import pandas as pd
ri= pd.read_csv('police.csv')
print(ri.head())
print(ri.isnull().sum())

print(ri.shape)
ri.drop(['county_name','state'], axis='columns', inplace=True)
print(ri.shape)

print(ri.isnull().sum())
ri.dropna(subset=['driver_gender'],inplace= True)
print(ri.isnull().sum())
print(ri.shape)
```
# Examing and Fixing the data types
```python
"""
Check the type of columns of the dataset.
Change those types into a suitable ones if necessary.
Args:
ri(DataFrame): the csvfile

Returns:
Figure out unmatching data types and change into another suitable ones.

print(ri.is_arrested.head())
ri['is_arrested']=ri.is_arrested.astype('bool')
print(ri.is_arrested.head())
"""
```
Column 'is_arrested' was changed from type "objected" to "bool" instead.


