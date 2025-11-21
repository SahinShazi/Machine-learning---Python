# Pandas DataFrame - Useful Methods & Data I/O

## Sample DataFrame

```python
import pandas as pd
import numpy as np

# Sample DataFrame তৈরি
df = pd.DataFrame({
    'Col1': np.random.randint(1, 100, 10),
    'Col2': np.random.randint(100, 500, 10),
    'Col3': ['Mike', 'Bob', 'Sara', 'Steve', 'Mike', 
             'Bob', 'Mike', 'Sara', 'Mike', 'Bob']
})

print(df)
```

**Output:**
```
   Col1  Col2   Col3
0    45   234   Mike
1    78   412    Bob
2    23   187   Sara
3    91   445  Steve
4    56   329   Mike
5    34   298    Bob
6    67   156   Mike
7    82   401   Sara
8    12   378   Mike
9    50   223    Bob
```

---

## .head() - প্রথম কয়েকটা Rows

বড় dataset এ সব data দেখা impossible। প্রথম কয়েকটা rows দিয়ে idea নিতে হয়।

### Default (5 rows)

```python
# First 5 rows
df.head()
```

**Output:**
```
   Col1  Col2  Col3
0    45   234  Mike
1    78   412   Bob
2    23   187  Sara
3    91   445 Steve
4    56   329  Mike
```

### Custom Number

```python
# First 10 rows
df.head(10)

# First 3 rows
df.head(3)
```

**Use Case:** 10,000+ rows থাকলে সব load না করে শুধু overview নেওয়া।

---

## .tail() - শেষ কয়েকটা Rows

```python
# Last 5 rows (default)
df.tail()

# Last 3 rows
df.tail(3)
```

**Output:**
```
   Col1  Col2  Col3
7    82   401  Sara
8    12   378  Mike
9    50   223   Bob
```

---

## .unique() - Unique Values

একটা column এ কতগুলো unique values আছে দেখতে:

```python
# Col3 এর unique values
unique_names = df['Col3'].unique()
print(unique_names)
```

**Output:**
```
['Mike' 'Bob' 'Sara' 'Steve']
```

**Returns:** NumPy array with unique values

---

## .nunique() - Count Unique Values

কতগুলো unique values আছে শুধু সংখ্যা:

```python
# Number of unique values
count = df['Col3'].nunique()
print(count)
```

**Output:**
```
4
```

---

## .value_counts() - Frequency Count

প্রতিটা unique value কতবার আছে:

```python
# Count each value
counts = df['Col3'].value_counts()
print(counts)
```

**Output:**
```
Col3
Mike     4
Bob      3
Sara     2
Steve    1
Name: count, dtype: int64
```

**Perfect!** Mike 4 বার, Bob 3 বার, Sara 2 বার, Steve 1 বার।

---

## .info() - Complete Information

DataFrame সম্পর্কে বিস্তারিত info:

```python
df.info()
```

**Output:**
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 10 entries, 0 to 9
Data columns (total 3 columns):
 #   Column  Non-Null Count  Dtype 
---  ------  --------------  ----- 
 0   Col1    10 non-null     int64 
 1   Col2    10 non-null     int64 
 2   Col3    10 non-null     object
dtypes: int64(2), object(1)
memory usage: 368.0+ bytes
```

**যা পাচ্ছো:**
- DataFrame type
- Index range (0 to 9)
- Total columns (3)
- Each column এর:
  - Name
  - Non-null count (missing values check)
  - Data type
- Memory usage

**খুবই important method!** প্রতিটা নতুন dataset এ প্রথমে এটা use করো।

---

## .describe() - Statistical Summary

Numerical columns এর statistics:

```python
df.describe()
```

**Output:**
```
            Col1        Col2
count  10.000000   10.000000
mean   53.800000  307.300000
std    26.455839  101.892045
min    12.000000  156.000000
25%    38.750000  248.250000
50%    53.000000  313.500000
75%    71.250000  394.750000
max    91.000000  445.000000
```

**একবারে পাচ্ছো:**
- Count
- Mean (average)
- Standard deviation
- Min, Max
- Quartiles (25%, 50%, 75%)

---

## .sort_values() - Sort Data

কোন column অনুযায়ী পুরো DataFrame sort করা:

```python
# Col2 অনুযায়ী sort
sorted_df = df.sort_values(by='Col2')
print(sorted_df)
```

**Output:**
```
   Col1  Col2   Col3
6    67   156   Mike
2    23   187   Sara
9    50   223    Bob
0    45   234  Mike
5    34   298    Bob
4    56   329   Mike
8    12   378   Mike
7    82   401   Sara
1    78   412    Bob
3    91   445  Steve
```

দেখো - Col2 ascending order এ sorted!

### Descending Order

```python
# Descending order
sorted_df = df.sort_values(by='Col2', ascending=False)
print(sorted_df)
```

### Multiple Columns

```python
# First by Col3, then by Col1
sorted_df = df.sort_values(by=['Col3', 'Col1'])
```

---

## .apply() - Custom Functions

নিজের function প্রতিটা element এ apply করা।

### Example 1: Multiply by 3

```python
# Custom function
def times_three(x):
    return x * 3

# Apply to Col2
result = df['Col2'].apply(times_three)
print(result)
```

**Output:**
```
0     702
1    1236
2     561
3    1335
4     987
5     894
6     468
7    1203
8    1134
9     669
Name: Col2, dtype: int64
```

প্রতিটা value 3 গুণ হয়ে গেছে!

### Example 2: Built-in Functions

```python
# String length
lengths = df['Col3'].apply(len)
print(lengths)
```

**Output:**
```
0    4  (Mike)
1    3  (Bob)
2    4  (Sara)
3    5  (Steve)
4    4  (Mike)
...
Name: Col3, dtype: int64
```

### Lambda Functions

```python
# Lambda দিয়ে
result = df['Col1'].apply(lambda x: x ** 2)
print(result)
```

**Output:**
```
0    2025  (45²)
1    6084  (78²)
2     529  (23²)
...
```

---

## Save DataFrame - to_csv()

DataFrame কে file এ save করা:

```python
# CSV file এ save
df.to_csv('test.csv')
```

এখন `test.csv` file তৈরি হয়ে গেছে!

### CSV File দেখতে:

```
,Col1,Col2,Col3
0,45,234,Mike
1,78,412,Bob
2,23,187,Sara
3,91,445,Steve
...
```

**CSV = Comma Separated Values**

---

## Load DataFrame - read_csv()

CSV file থেকে DataFrame load করা:

```python
# Load CSV
loaded_df = pd.read_csv('test.csv')
print(loaded_df)
```

**Output:**
```
   Unnamed: 0  Col1  Col2   Col3
0           0    45   234   Mike
1           1    78   412    Bob
2           2    23   187   Sara
...
```

### সমস্যা: Extra Column!

`Unnamed: 0` column এসেছে - এটা আসলে index ছিল।

### ✅ Solution: index_col Parameter

```python
# Index হিসেবে 0 column use করো
loaded_df = pd.read_csv('test.csv', index_col=0)
print(loaded_df)
```

**Output:**
```
   Col1  Col2   Col3
0    45   234   Mike
1    78   412    Bob
2    23   187   Sara
...
```

Perfect! Original DataFrame এর মতো।

---

## Save Options

### Without Index

```python
# Index বাদ দিয়ে save
df.to_csv('test.csv', index=False)
```

**CSV দেখতে:**
```
Col1,Col2,Col3
45,234,Mike
78,412,Bob
...
```

এখন load করতে `index_col` লাগবে না!

### Other Formats

```python
# Excel
df.to_excel('test.xlsx')

# JSON
df.to_json('test.json')

# HTML
df.to_html('test.html')

# Pickle (Python specific)
df.to_pickle('test.pkl')
```

---

## Read Options

```python
# Excel
pd.read_excel('test.xlsx')

# JSON
pd.read_json('test.json')

# HTML
pd.read_html('test.html')

# Pickle
pd.read_pickle('test.pkl')
```

---

## Practical Example

### Complete Workflow

```python
# 1. Create DataFrame
df = pd.DataFrame({
    'Product': ['Laptop', 'Phone', 'Tablet', 'Mouse', 'Keyboard'],
    'Price': [50000, 30000, 25000, 500, 1500],
    'Stock': [10, 25, 15, 100, 50]
})

# 2. Quick Overview
print(df.head())
print(df.info())

# 3. Check unique products
print(df['Product'].nunique())

# 4. Sort by price
df_sorted = df.sort_values(by='Price', ascending=False)
print(df_sorted)

# 5. Apply discount (10%)
df['Discounted_Price'] = df['Price'].apply(lambda x: x * 0.9)

# 6. Save
df.to_csv('products.csv', index=False)

# 7. Load later
loaded = pd.read_csv('products.csv')
print(loaded)
```

---

## Method Summary Table

| Method | কি করে | Returns |
|--------|--------|---------|
| `.head(n)` | First n rows | DataFrame |
| `.tail(n)` | Last n rows | DataFrame |
| `.info()` | Complete info | None (prints) |
| `.describe()` | Statistics | DataFrame |
| `.unique()` | Unique values | Array |
| `.nunique()` | Count unique | Integer |
| `.value_counts()` | Frequency | Series |
| `.sort_values()` | Sort data | DataFrame |
| `.apply(func)` | Custom function | Series/DataFrame |
| `.to_csv()` | Save to CSV | None |
| `pd.read_csv()` | Load CSV | DataFrame |

---

## Key Points

1. **`.head()`** - বড় dataset এর overview
2. **`.info()`** - সবসময় প্রথমে check করো
3. **`.unique()`** - Categorical data analysis
4. **`.value_counts()`** - Frequency distribution
5. **`.sort_values()`** - Data sorting
6. **`.apply()`** - Custom operations
7. **`.to_csv()`** - Data save করা
8. **`pd.read_csv()`** - Data load করা
9. **`index_col=0`** - Index load করতে
10. **`index=False`** - Index save না করতে

---

## Common Use Cases

### 1. Quick Data Check

```python
df.head()
df.info()
df.describe()
```

### 2. Categorical Analysis

```python
df['Category'].unique()
df['Category'].nunique()
df['Category'].value_counts()
```

### 3. Data Transformation

```python
df['Price'] = df['Price'].apply(lambda x: x * 1.15)  # 15% VAT
df['Name'] = df['Name'].apply(str.upper)  # Uppercase
```

### 4. Save & Load

```python
# Save
df.to_csv('data.csv', index=False)

# Load
df = pd.read_csv('data.csv')
```

---

## Best Practices

### ✅ DO

1. সবসময় `.head()` এবং `.info()` first
2. Large datasets এ `.head()` use করো
3. Save করার সময় `index=False` consider করো
4. Custom functions `.apply()` দিয়ে use করো
5. Regular backups `.to_csv()` দিয়ে

### ❌ DON'T

1. `.head()` ছাড়া পুরো dataset print করো না
2. Index handling ভুলে যেও না
3. `.apply()` তে function call `()` দিও না
4. Data save না করে long processing করো না

---

## Quick Reference

```python
# Overview
df.head()           # First 5 rows
df.tail()           # Last 5 rows
df.info()           # Complete info
df.describe()       # Statistics

# Column Analysis
df['col'].unique()          # Unique values
df['col'].nunique()         # Count unique
df['col'].value_counts()    # Frequency

# Sorting
df.sort_values(by='col')                    # Ascending
df.sort_values(by='col', ascending=False)   # Descending

# Apply
df['col'].apply(func)           # Custom function
df['col'].apply(lambda x: x*2)  # Lambda

# Save & Load
df.to_csv('file.csv', index=False)    # Save
pd.read_csv('file.csv')               # Load
```

---

## এখন কি জানো?

- ✅ DataFrame overview methods
- ✅ Unique values analysis
- ✅ Sorting data
- ✅ Custom functions apply করা
- ✅ CSV save এবং load করা
- ✅ Index handling

**পরবর্তী:**
- Real datasets নিয়ে কাজ
- Data visualization
- Advanced analysis

Happy Learning! 🚀