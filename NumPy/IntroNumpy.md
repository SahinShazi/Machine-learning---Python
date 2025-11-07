# NumPy - Numerical Python

## NumPy কি?

NumPy মানে Numerical Python। এটা Python এর একটা powerful library যেটা numbers নিয়ে কাজ করার জন্য বানানো। Machine Learning, Data Science, Scientific Computing - সব জায়গায় NumPy ব্যবহার হয়।

**কেন NumPy দরকার?**
- Python এর normal list থেকে অনেক দ্রুত
- Array নিয়ে কাজ করা সহজ
- Mathematical operations সহজ
- Memory efficient

## NumPy Array vs Python List

```python
# Python List
my_list = [1, 2, 3, 4, 5]

# NumPy Array
import numpy as np
my_array = np.array([1, 2, 3, 4, 5])
```

পার্থক্যটা কি? NumPy array এ সব element একই type এর হতে হয়, কিন্তু অনেক দ্রুত কাজ করে।

## Installation

```bash
pip install numpy
```

তারপর import করো:
```python
import numpy as np
```

সবাই `np` নাম দিয়ে import করে, এটা একটা standard practice।

---

## Problem 01: Range তৈরি করা

১ থেকে ১০ পর্যন্ত একটা array বানাতে হবে।

```python
import numpy as np

narry = np.arange(1, 10+1)
print(narry)
```

**Output:** `[1 2 3 4 5 6 7 8 9 10]`

**কি হচ্ছে:**
- `np.arange(1, 11)` মানে ১ থেকে শুরু, ১১ এর আগে শেষ
- Python এর `range()` এর মতোই কাজ করে
- কিন্তু এটা NumPy array return করে

আরেকটু দেখি:
```python
# ০ থেকে ৯
arr1 = np.arange(10)
print(arr1)  # [0 1 2 3 4 5 6 7 8 9]

# ৫ থেকে ১৫
arr2 = np.arange(5, 15)
print(arr2)  # [5 6 7 8 9 10 11 12 13 14]

# ২ করে বাড়াও (step)
arr3 = np.arange(0, 10, 2)
print(arr3)  # [0 2 4 6 8]
```

---

## Problem 02: Zeros Array

একটা ৫x৫ matrix যেখানে সব ০।

```python
zarry = np.zeros([5, 5])
print(zarry)
```

**Output:**
```
[[0. 0. 0. 0. 0.]
 [0. 0. 0. 0. 0.]
 [0. 0. 0. 0. 0.]
 [0. 0. 0. 0. 0.]
 [0. 0. 0. 0. 0.]]
```

**কাজের কথা:**
- `np.zeros()` সব ০ দিয়ে array বানায়
- `[5, 5]` মানে ৫ row, ৫ column
- Default float হয়ে থাকে (তাই ০.০ দেখাচ্ছে)

Integer চাইলে:
```python
zarry = np.zeros([5, 5], dtype=int)
print(zarry)
```

বিভিন্ন size:
```python
# ১ dimension
arr1 = np.zeros(5)
print(arr1)  # [0. 0. 0. 0. 0.]

# ৩x৪ matrix
arr2 = np.zeros([3, 4])
print(arr2)
```

---

## Problem 03: Ones Array

সব ১ দিয়ে ভরা ৪x৪ matrix।

```python
oarry = np.ones([4, 4])
print(oarry)
```

**Output:**
```
[[1. 1. 1. 1.]
 [1. 1. 1. 1.]
 [1. 1. 1. 1.]
 [1. 1. 1. 1.]]
```

zeros আর ones প্রায় একই। শুধু ১ আর ০ এর পার্থক্য।

মজার একটা কাজ - যেকোনো সংখ্যা দিয়ে ভরো:
```python
# ৫ দিয়ে ভরো
arr = np.ones([3, 3]) * 5
print(arr)

# অথবা সরাসরি
arr = np.full([3, 3], 5)
print(arr)
```

---

## Problem 04: Linspace - সমান ব্যবধানে

১ থেকে ৫ এর মধ্যে ১১টা সমান দূরত্বের সংখ্যা।

```python
liarry = np.linspace(start=1, stop=5, num=11)
print(liarry)
```

**Output:** `[1.  1.4 1.8 2.2 2.6 3.  3.4 3.8 4.2 4.6 5. ]`

**linspace vs arange:**
- `arange()` → step বলে দাও (কতটা করে বাড়বে)
- `linspace()` → কতগুলো চাই বলো

```python
# arange - step দিয়ে
arr1 = np.arange(0, 1, 0.1)  # ০.১ করে বাড়বে
print(arr1)

# linspace - সংখ্যা দিয়ে
arr2 = np.linspace(0, 1, 11)  # ১১টা point
print(arr2)
```

---

## Problem 05: List থেকে Array

Python list কে NumPy array তে convert করা।

```python
arr = [2, 6, 8, 7, 5, 9, 2, 9, 8, 4]
narr = np.array(arr)
print(narr)
```

**Output:** `[2 6 8 7 5 9 2 9 8 4]`

সহজ ব্যাপার। `np.array()` তে list টা দিয়ে দাও।

2D array ও বানাতে পারো:
```python
arr_2d = np.array([[1, 2, 3], [4, 5, 6]])
print(arr_2d)
# [[1 2 3]
#  [4 5 6]]

# Shape দেখো
print(arr_2d.shape)  # (2, 3) - ২ row, ৩ column
```

---

## Problem 06: Filter করা - জোড় সংখ্যা

০ থেকে ২০ পর্যন্ত শুধু জোড় সংখ্যা।

```python
narray = np.arange(21)
even_narray = narray[narray % 2 == 0]
print(even_narray)
```

**Output:** `[ 0  2  4  6  8 10 12 14 16 18 20]`

**এটা কিভাবে কাজ করছে?**
- `narray % 2 == 0` একটা boolean array তৈরি করে
- True যেখানে জোড়, False যেখানে বিজোড়
- সেই True জায়গার values নিয়ে নেয়

একটু দেখি:
```python
arr = np.array([1, 2, 3, 4, 5])
condition = arr > 2
print(condition)  # [False False  True  True  True]

result = arr[condition]
print(result)  # [3 4 5]
```

---

## Problem 07: সব element এ একসাথে operation

৩x৩ matrix এ সব ১, তারপর সব ৭ দিয়ে গুণ করো।

```python
array = np.ones([3, 3])
print(array * 7)
```

**Output:**
```
[[7. 7. 7.]
 [7. 7. 7.]
 [7. 7. 7.]]
```

NumPy এর সবচেয়ে powerful জিনিস! সবগুলো element এ একসাথে operation।

আরও examples:
```python
arr = np.array([1, 2, 3, 4, 5])

# যোগ
print(arr + 10)  # [11 12 13 14 15]

# বিয়োগ
print(arr - 2)   # [-1  0  1  2  3]

# গুণ
print(arr * 2)   # [ 2  4  6  8 10]

# ভাগ
print(arr / 2)   # [0.5 1.  1.5 2.  2.5]

# ঘাত
print(arr ** 2)  # [ 1  4  9 16 25]
```

---

## Problem 08: বিজোড় সংখ্যা

১ থেকে ৫০ এর মধ্যে সব বিজোড়।

```python
nrray = np.arange(1, 51)
odd_num = nrray[nrray % 2 != 0]
print(odd_num)
```

**Output:** `[ 1  3  5  7  9 ... 47 49]`

জোড় এর opposite - `!= 0` মানে ভাগশেষ ০ না হলে (মানে বিজোড়)।

আরেকটা উপায়:
```python
# সরাসরি বিজোড় দিয়ে শুরু, ২ করে বাড়াও
odd_num = np.arange(1, 51, 2)
print(odd_num)
```

---

## Problem 09: যোগফল বের করা

১ থেকে ১০০ পর্যন্ত সব সংখ্যার যোগফল।

```python
narray = np.arange(1, 101)
array = np.sum(narray)
print(array)
```

**Output:** `5050`

`np.sum()` সব element যোগ করে দেয়। খুব সহজ!

আরও কিছু useful functions:
```python
arr = np.array([1, 2, 3, 4, 5])

print(np.sum(arr))      # 15 (যোগফল)
print(np.mean(arr))     # 3.0 (গড়)
print(np.max(arr))      # 5 (সর্বোচ্চ)
print(np.min(arr))      # 1 (সর্বনিম্ন)
print(np.std(arr))      # Standard deviation
print(np.prod(arr))     # 120 (সবগুলো গুণফল)
```

---

## Problem 10: দ্বিগুণ করা

১ থেকে ৯ পর্যন্ত প্রতিটা সংখ্যাকে ২ দিয়ে গুণ।

```python
narray = np.arange(1, 10)
print(narray * 2)
```

**Output:** `[ 2  4  6  8 10 12 14 16 18]`

আগেই বলেছি, NumPy তে সব element এ একসাথে কাজ হয়। খুব সহজ!

---

## Problem 11: Reshape করা

১২টা সংখ্যাকে ৩x৪ matrix বানাও।

```python
narray = np.arange(12)
print(narray)  # [0 1 2 3 4 5 6 7 8 9 10 11]

rearr = narray.reshape([3, 4])
print(rearr)
```

**Output:**
```
[[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]]
```

**Reshape এর নিয়ম:**
- মোট element সংখ্যা same থাকতে হবে
- ১২টা element আছে → ৩x৪ = ১২ (কাজ করবে)
- ৩x৫ = ১৫ হলে error দিত

বিভিন্ন shape:
```python
arr = np.arange(24)

# ৪x৬
print(arr.reshape(4, 6))

# ২x১২
print(arr.reshape(2, 12))

# ৩ dimension
print(arr.reshape(2, 3, 4))

# Flatten (1D বানাও)
arr2d = np.array([[1, 2], [3, 4]])
print(arr2d.flatten())  # [1 2 3 4]
```

---

## Problem 12: ৩ দিয়ে বিভাজ্য

১ থেকে ১০ এর মধ্যে যেগুলো ৩ দিয়ে বিভাজ্য।

```python
narray = np.arange(1, 11)
reraay = narray[narray % 3 == 0]
print(reraay)
```

**Output:** `[3 6 9]`

Same filtering technique। `% 3 == 0` মানে ৩ দিয়ে ভাগ করলে ভাগশেষ ০।

Multiple conditions ও দিতে পারো:
```python
arr = np.arange(1, 21)

# ২ অথবা ৩ দিয়ে বিভাজ্য
result = arr[(arr % 2 == 0) | (arr % 3 == 0)]
print(result)  # [2 3 4 6 8 9 10 12 14 15 16 18 20]

# ২ এবং ৩ দুইটা দিয়েই বিভাজ্য (মানে ৬ দিয়ে)
result = arr[(arr % 2 == 0) & (arr % 3 == 0)]
print(result)  # [6 12 18]
```

**Note:** `|` মানে OR, `&` মানে AND

---

## Problem 13: Statistical Functions

Mean, Max, Min বের করা।

```python
num = np.array([10, 20, 30, 40, 50])

print("The mean:", np.mean(num), ", max:", np.max(num), "and min:", np.min(num))
```

**Output:** `The mean: 30.0 , max: 50 and min: 10`

NumPy তে অনেক statistical functions আছে:

```python
data = np.array([10, 20, 30, 40, 50])

print("Mean:", np.mean(data))           # 30.0 (গড়)
print("Median:", np.median(data))       # 30.0 (মধ্যমা)
print("Std Dev:", np.std(data))         # 14.14... (স্ট্যান্ডার্ড ডেভিয়েশন)
print("Variance:", np.var(data))        # 200.0 (ভেরিয়েন্স)
print("Sum:", np.sum(data))             # 150 (যোগফল)
print("Max:", np.max(data))             # 50
print("Min:", np.min(data))             # 10
print("Max Index:", np.argmax(data))    # 4 (সর্বোচ্চ এর index)
print("Min Index:", np.argmin(data))    # 0 (সর্বনিম্ন এর index)
```

---

## আরও কিছু Useful জিনিস

### Random Numbers

```python
# ০ থেকে ১ এর মধ্যে ৫টা random
arr = np.random.rand(5)
print(arr)

# ০ থেকে ১০০ এর মধ্যে ৫টা random integer
arr = np.random.randint(0, 100, 5)
print(arr)

# Normal distribution
arr = np.random.randn(5)
print(arr)
```

### Indexing এবং Slicing

```python
arr = np.array([10, 20, 30, 40, 50])

print(arr[0])        # 10 (প্রথম)
print(arr[-1])       # 50 (শেষ)
print(arr[1:4])      # [20 30 40] (index ১ থেকে ৩)
print(arr[::2])      # [10 30 50] (২ করে skip)

# 2D array
arr2d = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
print(arr2d[1, 2])   # 6 (row 1, col 2)
print(arr2d[:, 1])   # [2 5 8] (সব row এর column 1)
```

### Array Copy

```python
arr = np.array([1, 2, 3])

# Reference (একই memory)
arr2 = arr
arr2[0] = 999
print(arr)  # [999 2 3] - পরিবর্তন হয়ে গেছে!

# Copy (নতুন memory)
arr3 = arr.copy()
arr3[0] = 100
print(arr)  # [999 2 3] - unchanged
```

---

## NumPy কেন এত দ্রুত?

একটা comparison দেখি:

```python
import time

# Python List
start = time.time()
py_list = list(range(1000000))
py_result = [x * 2 for x in py_list]
print("Python List:", time.time() - start, "seconds")

# NumPy Array
start = time.time()
np_array = np.arange(1000000)
np_result = np_array * 2
print("NumPy Array:", time.time() - start, "seconds")
```

NumPy অনেক দ্রুত! কারণ:
- C language এ লেখা
- Vectorized operations (সব একসাথে)
- Memory efficient

---

## Common Mistakes

### ভুল ১: List এর মতো append করা

```python
# ❌ NumPy তে এটা কাজ করবে না সহজে
arr = np.array([1, 2, 3])
# arr.append(4)  # Error!

# ✅ এভাবে করতে হবে
arr = np.append(arr, 4)
print(arr)

# ✅ অথবা list বানিয়ে তারপর convert করো
my_list = [1, 2, 3]
my_list.append(4)
arr = np.array(my_list)
```

### ভুল ২: Reshape এ ভুল size

```python
arr = np.arange(10)  # ১০টা element

# ❌ Error - ১০টা কে ৩x৩ করা যাবে না
# arr.reshape(3, 3)

# ✅ ঠিক size দাও
arr.reshape(2, 5)  # বা (5, 2) বা (10, 1)
```

### ভুল ৩: Boolean indexing এ brackets

```python
arr = np.arange(10)

# ❌ ভুল - condition টা [] এর ভিতরে
result = arr[arr > 5]

# প্রথমে condition check করতে চাইলে
condition = arr > 5
result = arr[condition]
```

---

## Practice Problems

১. **১০০টা random সংখ্যা তৈরি করো, তারপর ৫০ এর বেশি কয়টা আছে count করো**

২. **১ থেকে ২০ পর্যন্ত একটা array বানাও, তারপর প্রাইম সংখ্যাগুলো খুঁজে বের করো**

৩. **দুইটা 3x3 matrix বানাও, তাদের যোগ, বিয়োগ, গুণ করো**

৪. **১ থেকে ১০০ পর্যন্ত সংখ্যা, square করো, তারপর যেগুলো ১০০০ এর বেশি সেগুলো বের করো**

৫. **একটা 5x5 identity matrix বানাও (diagonal এ ১, বাকি সব ০)**

---

NumPy শিখলে Data Science এর অর্ধেক কাজ শেষ! এটা foundation। Pandas, Matplotlib সব NumPy এর উপর build করা। Practice করতে থাকো, ধীরে ধীরে সব clear হবে।

Happy Coding! 🚀