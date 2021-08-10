# Pandas

## Data Types

### dtypes

DatetimeTZDtype, CategoricalDtype, PeriodDtype, SparseDtype, IntervalDtype, Int64Dtype, StringDtype, BooleanDtype

Get `Dataframe` dtypes for each column and value counter

```python
df.dtypes
df.dtypes.value_counts()
```

### defaults

By default integer types are int64 and float types are float64, regardless of platform (32-bit or 64-bit)

### astype

astype() method may convert dtypes from one to another. These will by default return a copy, even if the dtype was unchanged (pass copy=False to change this behavior).

Convert a subset of columns to a specified type using astype()

```python
df[["a", "b"]] = df[["a", "b"]].astype(np.uint8)
```

Convert certain columns to a specific dtype by passing a dict to astype()

```python
df = dfastype({"a": np.bool_, "c": np.float64})
```

note: loc() method tries to fit in what we are assigning to the current dtypes, while [] will overwrite them taking the dtype from the right hand side.

### object conversion

pandas has two ways to store strings.

1. object dtype, which can hold any Python object, including strings.
2. StringDtype, which is dedicated to strings.

The infer_objects() methods can be used to soft convert to the correct type.

```python
df.infer_objects().dtypes
```

- to_numeric() (conversion to numeric dtypes)
- to_datetime() (conversion to datetime objects)
- to_timedelta() (conversion to timedelta objects)

Convert Series datatype to numeric, getting rid of any non-numeric values  

```python
df['col'] = df['col'].astype(str).convert_objects(convert_numeric=True)
pd.to_datetime(df['col'], errors="ignore")
```

Convert Series datatype to numeric, changing  non-numeric values to NaN  

```python
df['col'] = pd.to_numeric(df['col'], errors='coerce')
```

The to_numeric() provides another argument `downcast`, which gives the option of downcasting the newly (or already) numeric data to a smaller dtype, which can conserve memory

```python
pd.to_numeric(m, downcast="integer")
```

As these methods apply only to one-dimensional arrays, lists or scalars; they cannot be used directly on multi-dimensional objects such as DataFrames. However, with apply(), we can `apply` the function over each column efficiently:

```python
df.apply(pd.to_datetime)
```

### Selecting columns based on dtype

select_dtypes() has two parameters include and exclude that allow you to say “give me the columns with these dtypes” (include) and/or “give the columns without these dtypes” (exclude).

```python
df.select_dtypes(include=["number", "bool"], exclude=["unsignedinteger"])
```

## Exploring and Finding Data

Get a report of all duplicate records in a DataFrame, based on specific columns

```python
dupes = df[df.duplicated(
    ['col1', 'col2', 'col3'], keep=False)]
```

List unique values in a DataFrame column  

```python
df['col'].unique()
```

For each unique value in a DataFrame column, get a frequency count

```python
df['col'].value_counts()
```

Grab DataFrame rows where column = a specific value

```python
df = df.loc[df.column == 'somevalue']
```

Grab DataFrame rows where column value is present in a list

```python
test_data = {'hi': 'yo', 'bye': 'later'}
df = pd.DataFrame(list(d.items()), columns=['col1', 'col2'])
valuelist = ['yo', 'heya']
df[df.col2.isin(valuelist)]
```

Grab DataFrame rows where column value is not present in a list

```python
test_data = {'hi': 'yo', 'bye': 'later'}
df = pd.DataFrame(list(d.items()), columns=['col1', 'col2'])
valuelist = ['yo', 'heya']
df[~df.col2.isin(valuelist)]
```

Select from DataFrame using criteria from multiple columns  
(use `|` instead of `&` to do an OR)

```python
newdf = df[(df['column_one']>2004) & (df['column_two']==9)]
```

Loop through rows in a DataFrame  
(if you must)

```python
for index, row in df.iterrows():
    print (index, row['some column'])
```

Much faster way to loop through DataFrame rows if you can work with tuples  

```python
for row in df.itertuples():
    print(row)
```

Get top n for each group of columns in a sorted DataFrame  
(make sure DataFrame is sorted first)

```python
top5 = df.groupby(
    ['groupingcol1',
    'groupingcol2']).head(5)
```

Grab DataFrame rows where specific column is null/notnull

```python
newdf = df[df['column'].isnull()]
```

Select from DataFrame using multiple keys of a hierarchical index

```python
df.xs(
    ('index level 1 value','index level 2 value'),
    level=('level 1','level 2'))
```

Slice values in a DataFrame column (aka Series)

```python
df.column.str[0:2]
```

Get quick count of rows in a DataFrame

```python
len(df.index)
```

Get length of data in a DataFrame column

```python
df.column_name.str.len()
```

## Updating and Cleaning Data

Delete column from DataFrame

```python
del df['column']
```

Rename several DataFrame columns

```python
df = df.rename(columns = {
    'col1 old name':'col1 new name',
    'col2 old name':'col2 new name',
    'col3 old name':'col3 new name',
})
```

Lower-case all DataFrame column names

```python
df.columns = map(str.lower, df.columns)
```

Even more fancy DataFrame column re-naming  
lower-case all DataFrame column names (for example)

```python
df.rename(columns=lambda x: x.split('.')[-1], inplace=True)
```

Lower-case everything in a DataFrame column

```python
df.column_name = df.column_name.str.lower()
```

Sort DataFrame by multiple columns

```python
df = df.sort_values(
    ['col1','col2','col3'],ascending=[1,1,0])
```

Change all NaNs to None (useful before loading to a db)

```python
df = df.where((pd.notnull(df)), None)
```

More pre-db insert cleanup...make a pass through the DataFrame, stripping whitespace from strings and changing any empty values to `None`  
(not especially recommended but including here b/c I had to do this in real life once)

```python
df = df.applymap(lambda x: str(x).strip() if len(str(x).strip()) else None)
```

Get rid of non-numeric values throughout a DataFrame:

```python
for col in refunds.columns.values:
  refunds[col] = refunds[col].replace(
      '[^0-9]+.-', '', regex=True)
 ```

Set DataFrame column values based on other column values  

```python
df.loc[(df['column1'] == some_value) & (df['column2'] == some_other_value), ['column_to_change']] = new_value
```

Clean up missing values in multiple DataFrame columns

```python
df = df.fillna({
    'col1': 'missing',
    'col2': '99.999',
    'col3': '999',
    'col4': 'missing',
    'col5': 'missing',
    'col6': '99'
})
```

Doing calculations with DataFrame columns that have missing values. In example below, swap in 0 for df['col1'] cells that contain null.

```python
df['new_col'] = np.where(
    pd.isnull(df['col1']), 0, df['col1']) + df['col2']
```

Split delimited values in a DataFrame column into two new columns  

```python
df['new_col1'], df['new_col2'] = zip(
    *df['original_col'].apply(
        lambda x: x.split(': ', 1)))
```

Collapse hierarchical column indexes

```python
df.columns = df.columns.get_level_values(0)
```

## Reshaping, Concatenating, and Merging Data

Pivot data (with flexibility about what what becomes a column and what stays a row).  

```python
pd.pivot_table(
  df,values='cell_value',
  index=['col1', 'col2', 'col3'], # these stay as columns; will fail silently if any of these cols have null values
  columns=['col4']) # data values in this column become their own column
```

Concatenate two DataFrame columns into a new, single column  
(useful when dealing with composite keys, for example)  

```python
df['newcol'] = df['col1'].astype(str) + df['col2'].astype(str)
```

## Display and formatting

Set up formatting so larger numbers aren't displayed in scientific notation  

```python
pd.set_option('display.float_format', lambda x: '%.3f' % x)
```

To display with commas and no decimals

```python
pd.options.display.float_format = '{:,.0f}'.format
```

## Creating DataFrames

Create a DataFrame from a Python dictionary

```python
df = pd.DataFrame(list(a_dictionary.items()), columns = ['column1', 'column2'])
```

List unique values in a DataFrame column  

```python
pd.unique(df.column_name.ravel())
```

Grab DataFrame rows where column has certain values

```python
valuelist = ['value1', 'value2', 'value3']
df = df[df.column.isin(valuelist)]
```

Grab DataFrame rows where column doesn't have certain values

```python
value_list = ['value1', 'value2', 'value3']
df = df[~df.column.isin(value_list)]
```

Delete column from DataFrame

```python
del df['column']
```

Select from DataFrame using criteria from multiple columns
    (use `|` instead of `&` to do an OR)

```python
newdf = df[(df['column_one'] > 2004) & (df['column_two'] == 9)]
```

Rename several DataFrame columns

```python
df = df.rename(columns={
    'col1 old name': 'col1 new name',
    'col2 old name': 'col2 new name',
    'col3 old name': 'col3 new name',
})
```

Lower-case all DataFrame column names

```python
df.columns = map(str.lower, df.columns)
```

Even more fancy DataFrame column re-naming
    lower-case all DataFrame column names (for example)

```python
df.rename(columns=lambda x: x.split('.')[-1], inplace=True)
```

Loop through rows in a DataFrame
    (if you must)

```python
for index, row in df.iterrows():
    print(index, row['some column'])
```

Much faster way to loop through DataFrame rows
    if you can work with tuples
    (h/t hughamacmullaniv)

```python
for row in df.itertuples():
    print(row)
```

Next few examples show how to work with text data in Pandas.
    [Full list of .str functions](http://pandas.pydata.org/pandas-docs/stable/text.html)
    Slice values in a DataFrame column (aka Series)

```python
df.column.str[0:2]
```

Lower-case everything in a DataFrame column

```python
df.column_name = df.column_name.str.lower()
```

Get length of data in a DataFrame column

```python
df.column_name.str.len()
```

Sort dataframe by multiple columns

```python
df = df.sort(['col1', 'col2', 'col3'], ascending=[1, 1, 0])
```

Get top n for each group of columns in a sorted dataframe
    (make sure dataframe is sorted first)

```python
top5 = df.groupby(['groupingcol1', 'groupingcol2']).head(5)
```

Grab DataFrame rows where specific column is null/notnull

```python
newdf = df[df['column'].isnull()]
```

Select from DataFrame using multiple keys of a hierarchical index

```python
df.xs(('index level 1 value', 'index level 2 value'), level=('level 1', 'level 2'))
```

Change all NaNs to None (useful before
loading to a db)

```python
df = df.where((pd.notnull(df)), None)
```

More pre-db insert cleanup...make a pass through the dataframe, stripping whitespace
    from strings and changing any empty values to None
    (not especially recommended but including here b/c I had to do this in real life one time)

```python
df = df.applymap(lambda x: str(x).strip() if len(str(x).strip()) else None)
```

Get quick count of rows in a DataFrame

```python
len(df.index)
```

Pivot data (with flexibility about what what
    becomes a column and what stays a row).
    Syntax works on Pandas >= .14

```python
pd.pivot_table(
  df,values='cell_value',
  index=['col1', 'col2', 'col3'], -these stay as columns; will fail silently if any of these cols have null values
  columns=['col4']) -data values in this column become their own column
```

Change data type of DataFrame column

```python
df.column_name = df.column_name.astype(np.int64)
```

Get rid of non-numeric values throughout a DataFrame:

```python
for col in refunds.columns.values:
  refunds[col] = refunds[col].replace('[^0-9]+.-', '', regex=True)
```

Set DataFrame column values based on other column values (h/t: @mlevkov)

```python
df.loc[(df['column1'] == some_value) & (df['column2'] == some_other_value), ['column_to_change']] = new_value
```

Clean up missing values in multiple DataFrame columns

```python
df = df.fillna({
    'col1': 'missing',
    'col2': '99.999',
    'col3': '999',
    'col4': 'missing',
    'col5': 'missing',
    'col6': '99'
})
```

Concatenate two DataFrame columns into a new, single column
    (useful when dealing with composite keys, for example)

```python
df['newcol'] = df['col1'].map(str) + df['col2'].map(str)
```

Doing calculations with DataFrame columns that have missing values
    In example below, swap in 0 for df['col1'] cells that contain null

```python
df['new_col'] = np.where(pd.isnull(df['col1']),0,df['col1']) + df['col2']
```

Split delimited values in a DataFrame column into two new columns

```python
df['new_col1'], df['new_col2'] = zip(*df['original_col'].apply(lambda x: x.split(': ', 1)))
```

Collapse hierarchical column indexes

```python
df.columns = df.columns.get_level_values(0)
```

Create a DataFrame from a Python dictionary

```python
df = pd.DataFrame(list(a_dictionary.items()), columns = ['column1', 'column2'])
```

Get a report of all duplicate records in a dataframe, based on specific columns

```python
dupes = df[df.duplicated(['col1', 'col2', 'col3'], keep=False)]
```

Set up formatting so larger numbers aren't displayed in scientific notation (h/t @thecapacity)

```python
pd.set_option('display.float_format', lambda x: '%.3f' % x)
```

## Pandas Code example

``` python
import pandas as pd
import numpy as np
from sqlalchemy import create_engine
engine = create_engine('mysql://root:xxx@127.0.0.1/lj?charset=utf8')
house = pd.read_sql_table('example', engine)
house_dd = pd.read_sql_query('select url, location, area, layout, size, buildtime, price, created from example group by url having count(url) > 1', engine)

def strip(text):
    try:
        return text.strip()
    except AttributeError:
        return text

def make_int(text):
    return int(text.strip('" '))

df = pd.DataFrame()
df1 = pd.read_csv('example.csv', sep=';')
df2 = pd.read_csv('new.csv', dtype=str, index_col=False,
                  usecols=['deal_num', 'acount', 'number'],
                  converters = {'deal_num' : strip,
                                    'acount' : strip,
                                    'number' : make_int})
```
