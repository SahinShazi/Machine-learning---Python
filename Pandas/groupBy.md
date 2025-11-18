# Pandas DataFrame - GroupBy Method

## GroupBy কি?

GroupBy হলো একটা powerful method যেটা data কে groups এ ভাগ করে, তারপর প্রতিটা group এ aggregation functions apply করে।

**Simple করে:**
- একটা column এর unique values অনুযায়ী group তৈরি হয়
- প্রতিটা group এ calculation করা হয় (sum, mean, max ইত্যাদি)
- Final result একসাথে পাওয়া যায়

**SQL জানো?** তাহলে `GROUP BY` clause মনে করো - একই জিনিস!

---

## Problem Statement

ধরো একটা pizza store এর data আছে:

```python
import pandas as pd

df = pd.DataFrame({
    'Store_Name': ['Dominos', 'Dominos', 'Pizza Hut', 'Pizza Hut', 
                   'Pizza Hut', 'Pizzaburg', 'Pizzalo'],
    'Location': ['Dhanmondi', 'Mirpur', 'Gulshan', 'Dhanmondi', 
                 'Mirpur', 'Uttara', 'Banani'],
    'Price': [450, 637, 550, 626, 651, 311, 375]
})

print(df)
```

**Output:**
```
   Store_Name   Location  Price
0     Dominos  Dhanmondi    450
1     Dominos     Mirpur    637
2   Pizza Hut    Gulshan    550
3   Pizza Hut  Dhanmondi    626
4   Pizza Hut     Mirpur    651
5   Pizzaburg     Uttara    311
6     Pizzalo     Banani    375
```

### Question: প্রতিটা store এর maximum price কত?

**Traditional way (without GroupBy):**
```python
# Dominos এর max
dominos_max = df[df['Store_Name'] == 'Dominos']['Price'].max()

# Pizza Hut এর max
pizzahut_max = df[df['Store_Name'] == 'Pizza Hut']['Price'].max()

# ... প্রতিটা store এর জন্য আলাদা করতে হবে!
```

অনেক code, অনেক repetition! 😓

---

## GroupBy দিয়ে Solution

### Basic GroupBy

```python
# Store_Name অনুযায়ী group করি
grouped = df.groupby('Store_Name')

# Maximum price দেখি
result = grouped.max()
print(result)
```

**Output:**
```
            Location  Price
Store_Name                 
Dominos       Mirpur    637
Pizza Hut     Mirpur    651
Pizzaburg     Uttara    311
Pizzalo       Banani    375
```

**একটা line এ সব store এর max price!** 🎉

---

## GroupBy কিভাবে কাজ করে?

### Step-by-Step Process

1. **Split** - Data কে groups এ ভাগ করো
2. **Apply** - প্রতিটা group এ function apply করো
3. **Combine** - Results একসাথে করো

```python
# Step 1: Group তৈরি
grouped = df.groupby('Store_Name')
print(type(grouped))
# <class 'pandas.core.groupby.generic.DataFrameGroupBy'>
```

এটা একটা **GroupBy object** - এখনো calculation হয়নি!

```python
# Step 2 & 3: Aggregation function apply
result = grouped.max()  # এখন calculation হবে
```

---

## Aggregation Functions

### Common Aggregations

```python
grouped = df.groupby('Store_Name')

# Maximum
print(grouped.max())

# Minimum
print(grouped.min())

# Mean (Average)
print(grouped.mean())

# Sum
print(grouped.sum())

# Count
print(grouped.count())
```

---

## numeric_only Parameter

### সমস্যা: String columns এ error

```python
# Mean calculate করি
result = df.groupby('Store_Name').mean()
```

**Error!** কারণ `Location` column string - এর mean হয় না!

### ✅ Solution: numeric_only=True

```python
# শুধু numeric columns এ apply হবে
result = df.groupby('Store_Name').mean(numeric_only=True)
print(result)
```

**Output:**
```
            Price
Store_Name       
Dominos     543.5
Pizza Hut   609.0
Pizzaburg   311.0
Pizzalo     375.0
```

Perfect! শুধু Price column এ calculation হয়েছে।

---

## String Columns এ Aggregation

### Max/Min String এ কি হয়?

```python
# সব columns এ max
result = df.groupby('Store_Name').max()
print(result)
```

**Output:**
```
            Location  Price
Store_Name                 
Dominos       Mirpur    637
Pizza Hut     Mirpur    651
Pizzaburg     Uttara    311
Pizzalo       Banani    375
```

**দেখো:** Location এ "Mirpur" দেখাচ্ছে, কিন্তু Dominos এর max price 637 যেটা Mirpur এ!

**কেন?** Lexicographically (alphabetically) "Mirpur" > "Dhanmondi"।

⚠️ **সতর্কতা:** String columns এ aggregation confusing results দিতে পারে!

### Sum String এ কি করে?

```python
# Sum without numeric_only
result = df.groupby('Store_Name').sum()
print(result)
```

**Output:**
```
            Location  Price
Store_Name                 
Dominos  DhanmondiMirpur   1087
Pizza Hut GulshanDhanmondiMirpur  1827
```

String concatenation হয়ে গেছে! 😅

**Best Practice:** সবসময় `numeric_only=True` use করো!

---

## .describe() Method

সব statistics একসাথে দেখতে চাও?

```python
grouped = df.groupby('Store_Name')
result = grouped.describe()
print(result)
```

**Output:**
```
           Price                                    
           count   mean         std    min     25%    50%     75%    max
Store_Name                                                              
Dominos      2.0  543.5   132.228  450.0  496.75  543.5  590.25  637.0
Pizza Hut    3.0  609.0    52.915  550.0  588.00  626.0  638.50  651.0
Pizzaburg    1.0  311.0      NaN  311.0  311.00  311.0  311.00  311.0
Pizzalo      1.0  375.0      NaN  375.0  375.00  375.0  375.00  375.0
```

**একসাথে পাচ্ছো:**
- Count (কতগুলো outlet)
- Mean (average price)
- Std (standard deviation)
- Min, 25%, 50%, 75%, Max

---

## Multiple Aggregations

একসাথে multiple calculations চাইলে `.agg()`:

```python
result = df.groupby('Store_Name').agg({
    'Price': ['mean', 'max', 'min', 'count']
})

print(result)
```

**Output:**
```
            Price                    
             mean  max  min count
Store_Name                        
Dominos     543.5  637  450     2
Pizza Hut   609.0  651  550     3
Pizzaburg   311.0  311  311     1
Pizzalo     375.0  375  375     1
```

---

## Practical Example

### Sales Data Analysis

```python
sales = pd.DataFrame({
    'Product': ['Laptop', 'Laptop', 'Phone', 'Phone', 'Tablet', 'Tablet'],
    'Region': ['Dhaka', 'Chittagong', 'Dhaka', 'Sylhet', 'Dhaka', 'Rajshahi'],
    'Sales': [50000, 45000, 30000, 28000, 25000, 22000],
    'Quantity': [5, 4, 10, 9, 8, 7]
})

print("Original Data:")
print(sales)
```

**Output:**
```
   Product      Region  Sales  Quantity
0   Laptop       Dhaka  50000         5
1   Laptop  Chittagong  45000         4
2    Phone       Dhaka  30000        10
3    Phone      Sylhet  28000         9
4   Tablet       Dhaka  25000         8
5   Tablet   Rajshahi  22000         7
```

### প্রতিটা Product এর total sales এবং average quantity:

```python
result = sales.groupby('Product').agg({
    'Sales': 'sum',
    'Quantity': 'mean'
})

print(result)
```

**Output:**
```
         Sales  Quantity
Product                 
Laptop   95000       4.5
Phone    58000       9.5
Tablet   47000       7.5
```

---

## Multiple Group Columns

একাধিক column দিয়ে group করতে পারো:

```python
# Product এবং Region দিয়ে group
result = sales.groupby(['Product', 'Region'])['Sales'].sum()
print(result)
```

**Output:**
```
Product  Region     
Laptop   Chittagong    45000
         Dhaka         50000
Phone    Dhaka         30000
         Sylhet        28000
Tablet   Dhaka         25000
         Rajshahi      22000
Name: Sales, dtype: int64
```

এটা একটা **MultiIndex Series**!

---

## Common Aggregation Functions

| Function | কি করে | Example |
|----------|--------|---------|
| `.sum()` | যোগফল | Total sales |
| `.mean()` | গড় | Average price |
| `.median()` | মধ্যমা | Median salary |
| `.min()` | সর্বনিম্ন | Lowest score |
| `.max()` | সর্বোচ্চ | Highest price |
| `.count()` | গণনা | Number of items |
| `.std()` | Standard deviation | Price variation |
| `.var()` | Variance | Data spread |
| `.first()` | প্রথম value | First entry |
| `.last()` | শেষ value | Last entry |

---

## GroupBy Workflow

### Best Practice Pattern

```python
# 1. Group object তৈরি করো (ভাগ ভাগ করে)
grouped = df.groupby('Store_Name')

# 2. Aggregation করো
max_price = grouped.max(numeric_only=True)
avg_price = grouped.mean(numeric_only=True)
total = grouped.sum(numeric_only=True)

# 3. Results analyze করো
print("Maximum Prices:")
print(max_price)

print("\nAverage Prices:")
print(avg_price)
```

---

## Key Points

1. **GroupBy** = Split → Apply → Combine
2. Group column এর **unique values** অনুযায়ী groups তৈরি হয়
3. **Aggregation functions** প্রতিটা group এ apply হয়
4. `numeric_only=True` use করো string column এর error এড়াতে
5. **GroupBy object** নিজে result না - calculation করতে method call করতে হয়
6. `.describe()` দিয়ে সব statistics একসাথে
7. `.agg()` দিয়ে multiple aggregations একসাথে
8. Multiple columns দিয়ে group করা যায়

---

## Common Mistakes

### ❌ Mistake 1: GroupBy object print করা

```python
grouped = df.groupby('Store_Name')
print(grouped)  # শুধু object address দেখাবে!
```

### ✅ Solution:

```python
grouped = df.groupby('Store_Name')
print(grouped.mean(numeric_only=True))  # Actual result
```

### ❌ Mistake 2: numeric_only ভুলে যাওয়া

```python
grouped.mean()  # Error যদি string column থাকে!
```

### ✅ Solution:

```python
grouped.mean(numeric_only=True)
```

---

## Documentation পড়ো! 📚

GroupBy এর অনেক advanced features আছে:
- Custom aggregation functions
- Transform methods
- Filter groups
- Apply custom logic
- Window functions

**Pandas Official Documentation:** 
https://pandas.pydata.org/docs/user_guide/groupby.html

⚠️ **মনে রাখো:** Tutorial শুধু 10-15% cover করে। বাকিটা documentation এ!

---

## Next Steps

এখন যা জানো:
- Basic GroupBy
- Common aggregations
- numeric_only parameter
- .describe() method

পরবর্তীতে শিখবো:
- Multiple group columns (detail এ)
- Custom aggregation functions
- Transform vs Aggregate
- Merging এবং Joining

---

## Quick Reference

```python
# Basic groupby
df.groupby('column').mean()
df.groupby('column').sum(numeric_only=True)
df.groupby('column').max(numeric_only=True)

# Multiple aggregations
df.groupby('column').agg(['mean', 'sum', 'count'])
df.groupby('column').agg({'col1': 'sum', 'col2': 'mean'})

# Multiple groups
df.groupby(['col1', 'col2']).sum()

# Statistics
df.groupby('column').describe()

# Store for reuse
grouped = df.groupby('column')
result1 = grouped.mean()
result2 = grouped.max()
```

Happy Learning! 🚀