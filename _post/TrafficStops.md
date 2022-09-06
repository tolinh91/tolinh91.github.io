# Introduction
On a typical day in the United States, police officers make more than 50,000 traffic stops.
The Stanford Open Policing Project is motivated for gathering, analyzing, and releasing records from millions of traffic stops by law enforcement agencies across 31 States of USA. 
Their goal is to help researchers, journalists, and policymakers investigate and improve interactions between police and the public.

The below is my practice for analyzing the data of traffic stops in the state of Rhode Island.
The related data is available in this [link](https://openpolicing.stanford.edu/data/). Please choose RI (State Patrol) to download.

```python
# Examining the data set
"""
Read the dataset into pandas.
Examine data using .isnull() and count the number of missing value by .isnull().sum().
Drop NA value using .dropna().
Args:
  ri(DataFrame): the csv file to read

Returns:
  DataFrame
"""

import pandas as pd
ri= pd.read_csv('police.csv')
ri.head(5)
ri.isnull().sum()
```


