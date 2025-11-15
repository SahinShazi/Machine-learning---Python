# Pandas DataFrame - পরিচিতি

## DataFrame কি?

DataFrame হলো Pandas এর সবচেয়ে important data structure। এটা একটা 2D table যেখানে rows এবং columns আছে - মানে Excel sheet বা database table এর মতো।

**Simple করে বললে:**
- Series = 1D (একটা column)
- DataFrame = 2D (পুরো table)

DataFrame আসলে multiple Series একসাথে - প্রতিটা column একটা Series।

---

## DataFrame তৈরি করা

### NumPy Array থেকে

```python
import numpy as np
import pandas as pd

# 6x5 random array
data = np.random.rand(6, 5)
print(data)

# DataFrame এ convert করি
df = pd.DataFrame(data=data)
print(df)
```

**Output:**
```
     0         1         2         3         4
0  0.234  0.567  0.123  0.789  0.456
1  0.891  0.234  0.678  0.345  0.901
2  0.456  0.789  0.234  0.678  0.345
3  0.123  0.456  0.789  0.234  0.567
4  0.678  0.345  0.901  0.567  0.234
5  0.234  0.678  0.345  0.789  0.456
```

দেখো - table format এ দেখাচ্ছে! বাম পাশে row index (0-5), উপরে column index (0-4)।

---

## Custom Row এবং Column Names

Default index এর বদলে নিজের মতো names দিতে পারো:

```python
data = np.random.rand(6, 5)

# Custom row এবং column names
df2 = pd.DataFrame(
    data=data,
    index=['Row1', 'Row2', 'Row3', 'Row4', 'Row5', 'Row6'],
    columns=['Col1', 'Col2', 'Col3', 'Col4', 'Col5']
)

print(df2)
```

**Output:**
```
         Col1      Col2      Col3      Col4      Col5
Row1  0.234  0.567  0.123  0.789  0.456
Row2  0.891  0.234  0.678  0.345  0.901
Row3  0.456  0.789  0.234  0.678  0.345
Row4  0.123  0.456  0.789  0.234  0.567
Row5  0.678  0.345  0.901  0.567  0.234
Row6  0.234  0.678  0.345  0.789  0.456
```

এখন row আর column এর meaningful names আছে!

---

## Column Select করা

### Single Column

```python
# Column 'Col1' নাও
col = df2['Col1']
print(col)
```

**Output:**
```
Row1    0.234
Row2    0.891
Row3    0.456
Row4    0.123
Row5    0.678
Row6    0.234
Name: Col1, dtype: float64
```

Series এর মতো দেখাচ্ছে, তাই না?

### এটা কি আসলেই Series?

```python
print(type(df2['Col1']))
# <class 'pandas.core.series.Series'>
```

হ্যাঁ! **প্রতিটা column একটা Series!**

DataFrame = Collection of Series (columns হিসেবে)

---

## Multiple Columns Select করা

List দিয়ে multiple columns নিতে পারো:

```python
# Col1, Col2, Col3 চাই
subset = df2[['Col1', 'Col2', 'Col3']]
print(subset)
```

**Output:**
```
         Col1      Col2      Col3
Row1  0.234  0.567  0.123
Row2  0.891  0.234  0.678
Row3  0.456  0.789  0.234
Row4  0.123  0.456  0.789
Row5  0.678  0.345  0.901
Row6  0.234  0.678  0.345
```

**Syntax মনে রাখো:** `df[['col1', 'col2']]` - list এর ভিতর list!

---

## নতুন Column Add করা

Dictionary এর মতোই কাজ করে:

```python
# নতুন column 'New' add করি
df2['New'] = np.random.rand(6)  # 6টা element (row সংখ্যা সমান)

print(df2)
```

**Output:**
```
         Col1      Col2      Col3      Col4      Col5       New
Row1  0.234  0.567  0.123  0.789  0.456  0.123
Row2  0.891  0.234  0.678  0.345  0.901  0.456
Row3  0.456  0.789  0.234  0.678  0.345  0.789
Row4  0.123  0.456  0.789  0.234  0.567  0.234
Row5  0.678  0.345  0.901  0.567  0.234  0.678
Row6  0.234  0.678  0.345  0.789  0.456  0.345
```

নতুন column শেষে add হয়ে গেছে!

### Important Rule

**Column length অবশ্যই row সংখ্যার সমান হতে হবে!**

```python
# ❌ Error দিবে - 5টা element কিন্তু 6টা row
# df2['Wrong'] = np.random.rand(5)

# ✅ সঠিক - 6টা element
df2['Correct'] = np.random.rand(6)
```

---

## DataFrame Structure

একটা DataFrame এর তিনটা main part:

1. **Data** - actual values
2. **Index** - row labels (বাম পাশে)
3. **Columns** - column labels (উপরে)

```python
df = pd.DataFrame(
    data=[[1, 2], [3, 4], [5, 6]],
    index=['A', 'B', 'C'],
    columns=['X', 'Y']
)
print(df)
```

**Output:**
```
   X  Y
A  1  2
B  3  4
C  5  6
```

---

## DataFrame = Multiple Series

এটা বুঝতে হবে:

```python
# DataFrame বানাই
df = pd.DataFrame({
    'Name': ['Rahim', 'Karim', 'Salma'],
    'Age': [25, 30, 28],
    'City': ['Dhaka', 'Chittagong', 'Sylhet']
})

# একটা column নিলাম
name_col = df['Name']

# Type check করি
print(type(name_col))  # Series
print(type(df))        # DataFrame
```

**Summary:**
- Each column → Series
- Full table → DataFrame

---

## Comparison: NumPy vs Pandas

### NumPy Array

```python
arr = np.array([[1, 2], [3, 4], [5, 6]])
print(arr)
# [[1 2]
#  [3 4]
#  [5 6]]
```

Organized না, কোনটা কি বুঝা মুশকিল।

### Pandas DataFrame

```python
df = pd.DataFrame(
    data=[[1, 2], [3, 4], [5, 6]],
    index=['A', 'B', 'C'],
    columns=['X', 'Y']
)
print(df)
#    X  Y
# A  1  2
# B  3  4
# C  5  6
```

Table format, row আর column names - organized আর readable!

---

## Practical Examples

### Example 1: Student Data

```python
students = pd.DataFrame({
    'Name': ['Rahim', 'Karim', 'Salma', 'Nadia'],
    'Math': [85, 92, 78, 95],
    'English': [78, 88, 92, 85],
    'Science': [90, 85, 88, 92]
})

print(students)
```

**Output:**
```
    Name  Math  English  Science
0  Rahim    85       78       90
1  Karim    92       88       85
2  Salma    78       92       88
3  Nadia    95       85       92
```

### Example 2: Sales Data

```python
sales = pd.DataFrame({
    'Product': ['Rice', 'Oil', 'Salt', 'Sugar'],
    'Price': [50, 120, 30, 45],
    'Quantity': [100, 50, 200, 80]
})

# নতুন column: Total
sales['Total'] = sales['Price'] * sales['Quantity']

print(sales)
```

**Output:**
```
  Product  Price  Quantity   Total
0    Rice     50       100    5000
1     Oil    120        50    6000
2    Salt     30       200    6000
3   Sugar     45        80    3600
```

---

## Key Points

1. **DataFrame = 2D table** with rows and columns
2. **Each column = Series** (1D)
3. Row labels = **Index**, Column labels = **Columns**
4. Custom names দেওয়া যায় rows আর columns কে
5. Column select: `df['col']` → Series
6. Multiple columns: `df[['col1', 'col2']]` → DataFrame
7. New column add: `df['new'] = values`
8. Column length = Row সংখ্যা (অবশ্যই!)

---

## Why DataFrame?

1. **Organized** - Table format, easy to read
2. **Labeled** - Row আর column names
3. **Flexible** - সহজে column add/remove করা যায়
4. **Powerful** - Data analysis এর জন্য অনেক features
5. **NumPy based** - Fast এবং efficient

DataFrame হলো Pandas এর heart। এটা ভালো করে বুঝলে data analysis অনেক সহজ হবে।

---

## Next Steps

এখন যা জানো:
- DataFrame তৈরি করা
- Custom index/columns
- Column select করা
- New column add করা

পরবর্তীতে শিখবো:
- Row selection (loc, iloc)
- Data filtering
- Sorting এবং grouping
- Real CSV data নিয়ে কাজ

Happy Coding! 🚀