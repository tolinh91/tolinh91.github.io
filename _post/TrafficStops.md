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
"""
print(ri.is_arrested.head())
ri['is_arrested']=ri.is_arrested.astype('bool')
print(ri.is_arrested.head())

combined = ri.stop_date.str.cat(ri.stop_time, sep=' ')
ri['stop_datetime']=pd.to_datetime(combined)
print(ri.dtypes)
ri.set_index('stop_datetime',inplace=True)
print(ri.index)
print(ri.columns)
```
Column 'is_arrested' was changed from type "objected" to "bool" instead.

# Does gender effect to violations?
When a driver is pulled over for speeding, many people believe that gender has an impact on whether the driver will receive a ticket or a warning. We will firgure out this assumption below.
##
```python
print(ri.violation.value_counts())
print(ri.violation.value_counts(normalize=True))
female=ri[ri['driver_gender']=='F']
male=ri[ri['driver_gender']=='M']
print(female.violation.value_counts(normalize=True))
print(male.violation.value_counts(normalize=True))
```

## Comparing speeding outcomes by gender
```python
"""
Compare the number of violations commited by female and male drivers.
Args:
ri (DataFrame): the csv file to read
female_and_speeding (DataFrame): female drivers who were stopped for speeding
male_and_speeding (DataFrame): male drivers who were stopped for speeding

Returns:
Count the stop outcomes for the female drivers and express them as proportions.
Count the stop outcomes for the male drivers and express them as proportions.
"""
female_and_speeding= ri[(ri['driver_gender']=="F") & (ri["violation_raw"]=="Speeding")]
male_and_speeding= ri[(ri['driver_gender']=="M") & (ri["violation_raw"]=="Speeding")]
print(female_and_speeding.stop_outcome.value_counts(normalize=True))
print(male_and_speeding.stop_outcome.value_counts(normalize=True))
# Check the data type of 'search_conducted'
print(ri.search_conducted.dtypes)

# Calculate the search rate by counting the values
print(ri.search_conducted.value_counts(normalize = True))

# Calculate the search rate by taking the mean
print(ri.search_conducted.mean())
ri[ri['driver_gender']=="F"].mean()
print(ri[ri['driver_gender']=="F"].search_conducted.mean())
print(ri[ri['driver_gender']=="M"].search_conducted.mean())
print(ri.groupby('driver_gender').search_conducted.mean())
# Reverse the ordering to group by violation before gender
print(ri.groupby(['violation','driver_gender']).search_conducted.mean())
# Count the 'search_type' values
print(ri.search_type.value_counts())

# Check if 'search_type' contains the string 'Protective Frisk'
ri['frisk'] = ri.search_type.str.contains('Protective Frisk', na= False)

# Check the data type of 'frisk'
print(ri.frisk.dtypes)

# Take the sum of 'frisk'
print(ri.frisk.sum())
# Create a DataFrame of stops in which a search was conducted
searched=ri[ri.search_conducted==True]

# Calculate the overall frisk rate by taking the mean of 'frisk'
print(searched.frisk.mean())

# Calculate the frisk rate for each gender
print(searched.groupby('driver_gender').frisk.mean())
```

