# Pandas DataFrame - Row এবং Column Operations

## Column/Row Remove করা - drop()

DataFrame থেকে row বা column remove করতে `drop()` method ব্যবহার করা হয়।

### Setup

```python
import pandas as pd
import numpy as np

# DataFrame তৈরি করি
data = np.random.rand(6, 5)
df2 = pd.DataFrame(
    data=data,
    index=['row_one', 'row_two', 'row_three', 'row_four', 'row_five', 'row_six'],
    columns=['col_one', 'col_two', 'col_three', 'col_four', 'col_five']
)

print(df2)
```

---

## Column Remove করা

### Temporary Remove

```python
# 'col_one' remove করি (temporary)
result = df2.drop(columns='col_one')
print(result)

# মূল DataFrame unchanged
print(df2)  # 'col_one' এখনো আছে
```

**গুরুত্বপূর্ণ:** `drop()` by default একটা **copy** return করে। Original DataFrame পরিবর্তন হয় না।

### Permanent Remove

```python
# Permanently remove করতে inplace=True
df2.drop(columns='col_one', inplace=True)
print(df2)  # এখন 'col_one' নেই
```

**inplace=True** মানে original DataFrame ই পরিবর্তন হবে।

### Multiple Columns Remove

```python
# একসাথে multiple columns
df2.drop(columns=['col_one', 'col_two'], inplace=True)
```

---

## Row Remove করা

Default behavior হলো row remove করা:

```python
# 'row_one' remove করি
result = df2.drop('row_one')
print(result)

# Original unchanged (inplace=True নেই)
print(df2)
```

### Permanent Row Remove

```python
# Permanently remove
df2.drop('row_one', inplace=True)
```

### Multiple Rows Remove

```python
# একসাথে multiple rows
df2.drop(['row_one', 'row_two'], inplace=True)
```

---

## Axis Parameter - গুরুত্বপূর্ণ Concept!

`axis` parameter দিয়ে বলা হয় কোন direction এ operation করবে।

### Axis বুঝা

```python
print(df2.shape)  # (6, 5)
# (rows, columns)
```

**Rule:**
- `axis=0` → **Rows** (vertical direction)
- `axis=1` → **Columns** (horizontal direction)

মনে রাখার টেকনিক:
- Shape এ প্রথম number (6) = axis 0 = rows
- Shape এ দ্বিতীয় number (5) = axis 1 = columns

### Column Remove করতে axis=1

```python
# এই দুইটা একই
df2.drop('col_one', axis=1)
df2.drop(columns='col_one')
```

### Row Remove করতে axis=0

```python
# এই দুইটা একই
df2.drop('row_one', axis=0)
df2.drop('row_one')  # default axis=0
```

---

## Row/Column Selection - loc[]

`loc` দিয়ে **label/name** ব্যবহার করে select করা যায়।

### Single Row Select

```python
# 'row_one' নাও
row = df2.loc['row_one']
print(row)
# Series return করবে
```

### Specific Cell (Row + Column)

```python
# row_one এর col_two value
value = df2.loc['row_one', 'col_two']
print(value)
# Single value return করবে
```

### Multiple Rows

```python
# row_one এবং row_two
rows = df2.loc[['row_one', 'row_two']]
print(rows)
# DataFrame return করবে
```

### Multiple Rows + Multiple Columns

```python
# Specific rows এর specific columns
subset = df2.loc[
    ['row_one', 'row_two'],           # rows
    ['col_three', 'col_five']         # columns
]
print(subset)
```

**Output:**
```
            col_three  col_five
row_one      0.234      0.789
row_two      0.567      0.345
```

### Row Slicing

```python
# row_one থেকে row_three পর্যন্ত
rows = df2.loc['row_one':'row_three']
# Note: শেষ label inclusive!
```

### Column Slicing

```python
# সব rows, কিন্তু col_two থেকে col_four
subset = df2.loc[:, 'col_two':'col_four']
```

---

## Integer Location - iloc[]

`iloc` দিয়ে **integer index** (position) ব্যবহার করে select করা যায়।

**Key difference:**
- `loc` → label/name দিয়ে
- `iloc` → integer position দিয়ে

### Single Cell

```python
# Row index 0, column index 1
value = df2.iloc[0, 1]
print(value)
```

### Row Selection

```python
# First row (index 0)
row = df2.iloc[0]

# First 3 rows
rows = df2.iloc[0:3]  # 0, 1, 2 (3 exclusive)
```

### Column Selection

```python
# সব rows, column index 2
col = df2.iloc[:, 2]

# সব rows, column 1-3
cols = df2.iloc[:, 1:4]  # 1, 2, 3
```

### Multiple Rows + Columns (Integer)

```python
# Row index 0,1 এবং column index 2,4
subset = df2.iloc[[0, 1], [2, 4]]
print(subset)
```

**Output:**
```
            col_three  col_five
row_one      0.234      0.789
row_two      0.567      0.345
```

### Slicing with iloc

```python
# Row 1-4, column 2-4
subset = df2.iloc[1:5, 2:5]
# Remember: end index exclusive!
```

---

## loc vs iloc - তুলনা

| Feature | loc | iloc |
|---------|-----|------|
| Selection by | Label/Name | Integer Position |
| Syntax | `df.loc['row', 'col']` | `df.iloc[0, 1]` |
| Slicing end | **Inclusive** | **Exclusive** |
| Example | `df.loc['A':'C']` includes C | `df.iloc[0:3]` excludes 3 |

### Example Comparison

```python
df = pd.DataFrame(
    data=[[1, 2], [3, 4], [5, 6]],
    index=['A', 'B', 'C'],
    columns=['X', 'Y']
)

# loc - label দিয়ে
print(df.loc['A', 'X'])  # 1

# iloc - position দিয়ে
print(df.iloc[0, 0])     # 1

# Slicing difference
print(df.loc['A':'C'])    # A, B, C (inclusive)
print(df.iloc[0:3])       # 0, 1, 2 (3 exclusive)
```

---

## Practical Examples

### Example 1: Student Data থেকে Column Remove

```python
students = pd.DataFrame({
    'Name': ['Rahim', 'Karim', 'Salma'],
    'Math': [85, 92, 78],
    'English': [78, 88, 92],
    'Temp': [0, 0, 0]  # unwanted column
})

# Remove 'Temp'
students.drop(columns='Temp', inplace=True)
print(students)
```

### Example 2: Specific Students এর Specific Subjects

```python
# Rahim আর Salma এর Math আর English
subset = students.loc[
    [0, 2],              # row indices
    ['Math', 'English']  # columns
]
# বা
subset = students.iloc[[0, 2], [1, 2]]
```

### Example 3: First 3 Rows Remove

```python
# প্রথম 3 rows remove
df.drop(df.index[:3], inplace=True)
```

---

## Common Patterns

### Pattern 1: Last Row Remove

```python
# শেষ row remove
df.drop(df.index[-1], inplace=True)
```

### Pattern 2: Columns Except One

```python
# 'Name' ছাড়া সব columns
cols_to_drop = [col for col in df.columns if col != 'Name']
df.drop(columns=cols_to_drop, inplace=True)
```

### Pattern 3: Conditional Row Removal

```python
# যেসব row এ Math < 80
to_drop = students[students['Math'] < 80].index
students.drop(to_drop, inplace=True)
```

---

## Important Notes

### 1. inplace Parameter

```python
# Without inplace (returns copy)
new_df = df.drop(columns='col')  # df unchanged

# With inplace (modifies original)
df.drop(columns='col', inplace=True)  # df changed
```

### 2. Drop Non-existent Column/Row

```python
# Error দিবে
# df.drop(columns='nonexistent')

# Safely drop (errors='ignore')
df.drop(columns='nonexistent', errors='ignore')
```

### 3. Reset Index After Drop

```python
df.drop([0, 2, 4], inplace=True)
print(df.index)  # [1, 3, 5] - gaps আছে

# Reset করো
df.reset_index(drop=True, inplace=True)
print(df.index)  # [0, 1, 2] - continuous
```

---

## Summary

### Drop Operations
- `df.drop(columns='col')` → column remove
- `df.drop('row')` → row remove
- `inplace=True` → permanent change

### Axis
- `axis=0` → rows
- `axis=1` → columns

### Selection
- `df.loc['row', 'col']` → label দিয়ে
- `df.iloc[0, 1]` → integer position দিয়ে

### Key Difference
- `loc` slicing: **inclusive** end
- `iloc` slicing: **exclusive** end

এই operations গুলো master করলে DataFrame manipulation অনেক সহজ হবে!

Happy Coding! 🚀