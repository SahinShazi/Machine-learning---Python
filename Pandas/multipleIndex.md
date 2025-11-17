# Pandas DataFrame - MultiIndex (Hierarchical Indexing)

## MultiIndex কি?

MultiIndex হলো DataFrame এর একটা advanced feature যেখানে একাধিক level এর index থাকে। এটাকে **Hierarchical Indexing** ও বলা হয়।

**Simple করে বললে:**
- Normal DataFrame → একটা index (Row1, Row2, Row3...)
- MultiIndex DataFrame → একাধিক nested index (Group1 → Row1, Row2, Row3...)

**কখন লাগে?**
- Grouped data represent করতে
- Hierarchical structure maintain করতে
- Complex data organization এর জন্য

⚠️ **Note:** এটা একটা rare use case। বেশিরভাগ সময় normal DataFrame-ই যথেষ্ট।

---

## MultiIndex তৈরি করা

### Step 1: Index Structure Setup

প্রথমে আমরা hierarchical structure এর জন্য data তৈরি করব:

```python
import numpy as np
import pandas as pd

# Outer level: Groups (3টা Group1 এবং 3টা Group2)
groups = ['Group1'] * 3 + ['Group2'] * 3

# Inner level: Rows (দুইবার Row1, Row2, Row3)
inside_groups = ['Row1', 'Row2', 'Row3'] * 2

# Column names
columns = ['Col1', 'Col2']

print("Groups:", groups)
print("Inside Groups:", inside_groups)
```

**Output:**
```
Groups: ['Group1', 'Group1', 'Group1', 'Group2', 'Group2', 'Group2']
Inside Groups: ['Row1', 'Row2', 'Row3', 'Row1', 'Row2', 'Row3']
```

### কিভাবে তৈরি হলো?

```python
# এটা বুঝতে ভাগ ভাগ করে দেখি
groups_explained = ['Group1'] * 3  # ['Group1', 'Group1', 'Group1']
groups_explained2 = ['Group2'] * 3  # ['Group2', 'Group2', 'Group2']
groups_full = groups_explained + groups_explained2  # দুটো যোগ

# Inside groups
inside = ['Row1', 'Row2', 'Row3'] * 2  # দুইবার repeat
```

দেখো - list multiplication দিয়ে সহজেই pattern তৈরি করা যায়!

---

## zip() দিয়ে Pairing করা

### zip() Function কি?

`zip()` দুটো list এর elements কে one-to-one pair করে tuple আকারে দেয়।

```python
# Groups এবং Inside Groups কে pair করি
ind_tuple = list(zip(groups, inside_groups))
print(ind_tuple)
```

**Output:**
```
[('Group1', 'Row1'),
 ('Group1', 'Row2'),
 ('Group1', 'Row3'),
 ('Group2', 'Row1'),
 ('Group2', 'Row2'),
 ('Group2', 'Row3')]
```

**Perfect!** প্রতিটা Group এর সাথে তার corresponding Rows pair হয়ে গেছে।

---

## MultiIndex Object তৈরি

### pd.MultiIndex.from_tuples()

এখন tuples থেকে MultiIndex object তৈরি করি:

```python
# MultiIndex তৈরি
index = pd.MultiIndex.from_tuples(ind_tuple)
print(index)
```

**Output:**
```
MultiIndex([('Group1', 'Row1'),
            ('Group1', 'Row2'),
            ('Group1', 'Row3'),
            ('Group2', 'Row1'),
            ('Group2', 'Row2'),
            ('Group2', 'Row3')],
           )
```

এটা একটা special index object যেটা hierarchical structure বুঝে!

---

## MultiIndex DataFrame তৈরি

### Complete DataFrame

```python
# DataFrame তৈরি
df = pd.DataFrame(
    data=np.random.randn(6, 2),  # 6 rows, 2 columns
    index=index,
    columns=columns
)

print(df)
```

**Output:**
```
                Col1      Col2
Group1 Row1  0.234  -0.567
       Row2 -0.891   0.234
       Row3  0.456  -0.789
Group2 Row1 -0.123   0.456
       Row2  0.678  -0.345
       Row3 -0.234   0.678
```

দেখো - দুই level এর index! 
- **Outer level:** Group1, Group2
- **Inner level:** Row1, Row2, Row3

---

## MultiIndex থেকে Data Select করা

### সমস্যা: Normal Indexing কাজ করে না

```python
# ❌ এটা কাজ করবে না directly
# df.loc['Row1']  # Error!
```

**কেন?** MultiIndex এ level-by-level যেতে হয়!

### Level-by-Level Selection

#### Method 1: Outer Level Select

```python
# Group1 এর সব data
group1 = df.loc['Group1']
print(group1)
```

**Output:**
```
         Col1      Col2
Row1  0.234  -0.567
Row2 -0.891   0.234
Row3  0.456  -0.789
```

#### Method 2: Nested .loc

```python
# Group1 এর Row1
result = df.loc['Group1'].loc['Row1']
print(result)
```

**Output:**
```
Col1    0.234
Col2   -0.567
Name: Row1, dtype: float64
```

#### Method 3: Chaining .loc (One-liner)

```python
# একসাথে দুই level
result = df.loc['Group1'].loc['Row1']
print(result)
```

**Rule:** বাইরে থেকে ভেতরে - প্রতিটা level একটার পর একটা!

---

## Index Names সেট করা

### কেন Names দরকার?

MultiIndex এর প্রতিটা level কে identify করতে names দেওয়া যায়। এটা পরে `.xs()` method এ কাজে লাগে।

```python
# Index names চেক করি (আগে)
print(df.index.names)
# Output: FrozenList([None, None])

# Names সেট করি
df.index.names = ['Groups', 'Rows']

# এখন দেখি
print(df)
```

**Output:**
```
Groups Rows      Col1      Col2
Group1 Row1   0.234  -0.567
       Row2  -0.891   0.234
       Row3   0.456  -0.789
Group2 Row1  -0.123   0.456
       Row2   0.678  -0.345
       Row3  -0.234   0.678
```

এখন index এর label দেখাচ্ছে: **Groups** এবং **Rows**!

---

## .xs() Method - Cross Section

### .xs() কি?

`.xs()` (cross-section) হলো MultiIndex এর জন্য special method যা **level skip** করতে পারে!

**`.loc` vs `.xs()`:**

| Feature | .loc | .xs() |
|---------|------|-------|
| Type | Attribute | Method |
| Syntax | `df.loc['value']` | `df.xs('value')` |
| Level Skip | ❌ পারে না | ✅ পারে |
| Brackets | Square `[]` | Round `()` |

### Basic Usage

```python
# Group1 এর সব data
result = df.xs('Group1')
print(result)
```

**Output:**
```
         Col1      Col2
Row1  0.234  -0.567
Row2 -0.891   0.234
Row3  0.456  -0.789
```

⚠️ **মনে রাখো:** `.xs()` একটা **method** (round brackets), `.loc` একটা **attribute** (square brackets)!

---

## Level Skip করা

### সমস্যা: সব Group থেকে Row1 চাই

Normal `.loc` দিয়ে সম্ভব না - outer level skip করতে পারবে না।

**Solution: `.xs()` with `level` parameter**

```python
# ❌ এটা error দিবে (outer level এ Row1 নেই)
# df.xs('Row1')  # KeyError

# ✅ সঠিক পদ্ধতি - level specify করি
result = df.xs('Row1', level='Rows')
print(result)
```

**Output:**
```
           Col1      Col2
Groups                   
Group1  0.234  -0.567
Group2 -0.123   0.456
```

**Perfect!** দুই Group থেকেই Row1 পেয়ে গেছি!

### এটা কিভাবে কাজ করে?

1. `df.xs('Row1')` → Default outer level (Groups) এ খুঁজবে → পাবে না
2. `df.xs('Row1', level='Rows')` → 'Rows' level এ খুঁজবে → পাবে!

---

## Practical Examples

### Example 1: Company Departments Data

```python
# Departments এবং Employees
depts = ['Sales'] * 3 + ['IT'] * 3
employees = ['Emp1', 'Emp2', 'Emp3'] * 2

# MultiIndex তৈরি
tuples = list(zip(depts, employees))
index = pd.MultiIndex.from_tuples(tuples)

# DataFrame
company = pd.DataFrame(
    data={
        'Salary': [50000, 55000, 60000, 70000, 75000, 80000],
        'Age': [25, 30, 35, 28, 32, 40]
    },
    index=index
)

# Index names
company.index.names = ['Department', 'Employee']
print(company)
```

**Output:**
```
Department Employee  Salary  Age
Sales      Emp1      50000   25
           Emp2      55000   30
           Emp3      60000   35
IT         Emp1      70000   28
           Emp2      75000   32
           Emp3      80000   40
```

### Example 2: Specific Queries

```python
# Sales department এর সব employees
sales = company.xs('Sales')
print(sales)

# সব department থেকে Emp1
all_emp1 = company.xs('Emp1', level='Employee')
print(all_emp1)

# Sales এর Emp2
sales_emp2 = company.loc['Sales'].loc['Emp2']
print(sales_emp2)
```

---

## MultiIndex Levels বুঝা

### Level Structure

```python
# Levels check করি
print(df.index.levels)
# Output: [Index(['Group1', 'Group2']), Index(['Row1', 'Row2', 'Row3'])]

# কতগুলো level?
print(df.index.nlevels)
# Output: 2
```

**Visualization:**
```
Level 0 (Groups):     Group1              Group2
                      ↓                   ↓
Level 1 (Rows):    Row1 Row2 Row3    Row1 Row2 Row3
```

---

## Common Use Cases

### 1. Time Series with Groups

```python
dates = pd.date_range('2024-01-01', periods=3)
groups = ['Product_A', 'Product_B']

# সব combination
index = pd.MultiIndex.from_product([groups, dates], 
                                   names=['Product', 'Date'])
```

### 2. Geographic Data

```python
countries = ['Bangladesh', 'India']
cities = ['Dhaka', 'Chittagong', 'Delhi', 'Mumbai']

# Manual tuples
tuples = [
    ('Bangladesh', 'Dhaka'),
    ('Bangladesh', 'Chittagong'),
    ('India', 'Delhi'),
    ('India', 'Mumbai')
]
index = pd.MultiIndex.from_tuples(tuples, names=['Country', 'City'])
```

---

## Advanced Selection Techniques

### Slicing in MultiIndex

```python
# Group1 এর Row1 থেকে Row2
result = df.loc[('Group1', 'Row1'):('Group1', 'Row2')]
print(result)
```

### Boolean Indexing

```python
# Col1 > 0 এর সব rows
result = df[df['Col1'] > 0]
print(result)
```

### Combining .xs() with .loc

```python
# সব Group থেকে Row1, শুধু Col1
result = df.xs('Row1', level='Rows')['Col1']
print(result)
```

**Output:**
```
Groups
Group1    0.234
Group2   -0.123
Name: Col1, dtype: float64
```

---

## MultiIndex থেকে Normal Index এ Convert

### .reset_index()

```python
# MultiIndex remove করি
df_normal = df.reset_index()
print(df_normal)
```

**Output:**
```
   Groups  Rows      Col1      Col2
0  Group1  Row1  0.234  -0.567
1  Group1  Row2 -0.891   0.234
2  Group1  Row3  0.456  -0.789
3  Group2  Row1 -0.123   0.456
4  Group2  Row2  0.678  -0.345
5  Group2  Row3 -0.234   0.678
```

এখন Groups আর Rows regular columns হয়ে গেছে!

---

## সুবিধা এবং অসুবিধা

### ✅ সুবিধা

1. **Hierarchical data** সুন্দরভাবে represent করা যায়
2. **Grouping** natural ভাবে maintain হয়
3. **Memory efficient** - repeated labels save করে না
4. **Advanced slicing** সম্ভব

### ❌ অসুবিধা

1. **Complex** - শুরুতে confusing
2. **Syntax** different - `.xs()`, level parameters
3. **Rare use** - বেশিরভাগ সময় দরকার হয় না
4. **Debugging** কঠিন হতে পারে

---

## Key Points

1. **MultiIndex** = একাধিক level এর index
2. **from_tuples()** দিয়ে tuples থেকে তৈরি করা যায়
3. **Level-by-level** selection করতে হয় (বাইরে থেকে ভেতরে)
4. **Index names** সেট করা best practice
5. **`.loc`** = attribute (square brackets `[]`)
6. **`.xs()`** = method (round brackets `()`)
7. `.xs()` দিয়ে **level skip** করা যায়
8. `level` parameter দিয়ে specific level select করা যায়
9. **Rare use case** - normal DataFrame-ই বেশি ব্যবহার হয়
10. **`.reset_index()`** দিয়ে normal DataFrame এ convert করা যায়

---

## Common Mistakes

### ❌ Mistake 1: .xs() কে Attribute মনে করা

```python
# Wrong
df.xs['Row1']  # Error! .xs is a method

# Correct
df.xs('Row1')
```

### ❌ Mistake 2: Level Skip করার চেষ্টা (without level parameter)

```python
# Wrong - outer level এ Row1 নেই
df.xs('Row1')  # KeyError!

# Correct
df.xs('Row1', level='Rows')
```

### ❌ Mistake 3: Index Names সেট না করা

```python
# Names না থাকলে level number use করতে হয়
df.xs('Row1', level=1)  # Confusing!

# Names থাকলে clear
df.xs('Row1', level='Rows')  # Better!
```

---

## Quick Reference

```python
# MultiIndex তৈরি
tuples = [('A', 1), ('A', 2), ('B', 1), ('B', 2)]
index = pd.MultiIndex.from_tuples(tuples, names=['Letter', 'Number'])

# DataFrame তৈরি
df = pd.DataFrame(data, index=index, columns=cols)

# Selection methods
df.loc['A']                      # Outer level
df.loc['A'].loc[1]              # Nested
df.xs('A')                       # Cross-section outer
df.xs(1, level='Number')        # Cross-section inner (level skip)
df.xs(1, level=1)               # Level by position

# Convert to normal
df.reset_index()                # MultiIndex → Columns
```

---

## Practice Exercise

নিজে একটা MultiIndex DataFrame বানাও:

1. ✅ 2টা School (School_A, School_B)
2. ✅ প্রতি School এ 3টা Student (Student1, Student2, Student3)
3. ✅ 2টা Column (Math, Science)
4. ✅ Random marks দাও
5. ✅ Index names সেট করো
6. ✅ School_A এর সব students নাও
7. ✅ সব School থেকে Student1 নাও
8. ✅ `.xs()` ব্যবহার করো

```python
# তোমার code এখানে লিখো
schools = ['School_A'] * 3 + ['School_B'] * 3
students = ['Student1', 'Student2', 'Student3'] * 2

# ... বাকি code তুমি complete করো!
```

---

## When to Use MultiIndex?

### ✅ Use করো যখন:
- Naturally hierarchical data (Country → City → Store)
- Time series with groups (Product → Date)
- Pivot table style data
- Memory constraint আছে (repeated labels save)

### ❌ এড়িয়ে চলো যখন:
- Simple flat data structure যথেষ্ট
- Frequent column-wise operations দরকার
- Sharing data with non-Pandas users
- Visualization এর জন্য (normal format better)

---

## Real-World Reality

**সত্যি কথা:** 95%+ cases এ normal DataFrame ই যথেষ্ট। MultiIndex খুবই rare!

**আমার অভিজ্ঞতা:**
- হাজারো DataFrame নিয়ে কাজ করেছি
- মাত্র 2-3টা genuine MultiIndex case দেখেছি
- সেগুলোও পরে normal DataFrame এ convert করেছি

**তাহলে কেন শিখছি?**
- যদি দেখো তাহলে ঘাবড়াবে না! 😊
- Advanced Pandas concept বুঝতে পারবে
- Interview questions এ আসতে পারে

---

## Next Steps

এখন যা জানো:
- MultiIndex structure
- `.loc` vs `.xs()`
- Level-by-level selection
- Index names
- Level skipping

পরবর্তীতে শিখবো:
- `.stack()` এবং `.unstack()`
- `.pivot()` এবং `.pivot_table()`
- GroupBy with MultiIndex
- Advanced reshaping

**Practice করো!** একটা বড় random MultiIndex DataFrame বানাও আর experiment করো। 🚀

Happy Learning! 🎓