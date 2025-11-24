# Matplotlib Subplots এবং Legends

## Subplots কি?

Subplots হলো একটা figure এর ভেতরে একাধিক আলাদা আলাদা graph তৈরি করার পদ্ধতি। Object-oriented approach ব্যবহার করে সহজেই subplots তৈরি করা যায়।

---

## `plt.subplots()` Method

### Basic Syntax

```python
import matplotlib.pyplot as plt
import numpy as np

# Figure এবং Axes object তৈরি
fig, axes = plt.subplots()
```

**Output:**
```
একটা empty canvas দেখাবে
```

এই method দুইটা object return করে:
- `fig` - Figure object (canvas)
- `axes` - Axes object (plotting area)

### Single Plot তৈরি

যদি কোনো parameter না দেওয়া হয়, তাহলে একটা single plot তৈরি হয়:

```python
# Sample data
x = np.linspace(0, 10, 100)
y = np.sin(x)

# Figure এবং axes তৈরি
fig, axes = plt.subplots()

# Plot করা
axes.plot(x, y)

# দেখানো
fig
```

**Output:**
```
একটা sine wave এর line plot দেখাবে
```

এটা আগের video তে দেখানো `fig.add_axes()` method এর মতোই কাজ করে, কিন্তু এখানে directly figure এবং axes পাওয়া যায়।

---

## Multiple Subplots তৈরি

### Rows এবং Columns Define করা

`nrows` এবং `ncols` parameter ব্যবহার করে একাধিক subplot তৈরি করা যায়।

```python
# 1 row, 3 columns এ subplots তৈরি
fig, axes = plt.subplots(nrows=1, ncols=3)

fig
```

**Output:**
```
একটা figure এ পাশাপাশি 3টা empty subplot দেখাবে
```

### Default Size এর সমস্যা

Default size এ text overlap হতে পারে এবং দেখতে ভালো লাগে না।

---

## Figure Size Control করা

### `figsize` Parameter

Figure এর size control করার জন্য `figsize` parameter ব্যবহার করতে হয়। এটা একটা hidden parameter যা internally `plt.figure()` method থেকে আসে।

```python
# figsize দিয়ে size set করা
fig, axes = plt.subplots(nrows=1, ncols=3, figsize=(15, 5))

fig
```

**Output:**
```
Width বড় হয়ে একটা horizontal figure দেখাবে
```

**Syntax:**
```python
figsize=(width, height)
```

- প্রথম value = Width
- দ্বিতীয় value = Height

### Size Adjustment Examples

```python
# Width ছোট করা
fig, axes = plt.subplots(nrows=1, ncols=3, figsize=(10, 5))
```

```python
# Height বড় করা
fig, axes = plt.subplots(nrows=1, ncols=3, figsize=(15, 10))
# দেখতে অনেক বড় লাগবে
```

```python
# Balanced size
fig, axes = plt.subplots(nrows=1, ncols=3, figsize=(15, 5))
# এটা সবচেয়ে ভালো দেখায়
```

---

## Axes Array এবং Indexing

Multiple subplots তৈরি করলে `axes` একটা NumPy array হয়ে যায়।

### Array Structure

```python
fig, axes = plt.subplots(nrows=1, ncols=3, figsize=(15, 5))

# axes এখন একটা NumPy array
print(type(axes))
# <class 'numpy.ndarray'>

print(axes)
# [<Axes 1>, <Axes 2>, <Axes 3>]
```

প্রতিটা subplot এর জন্য একটা separate axes object থাকে।

### Indexing দিয়ে Plot করা

```python
# Sample data
x = np.linspace(0, 10, 100)
y = np.sin(x)

# 3 subplots তৈরি
fig, axes = plt.subplots(nrows=1, ncols=3, figsize=(15, 5))

# তৃতীয় subplot এ plot (index 2)
axes[2].plot(x, y)

fig
```

**Output:**
```
শেষের (third) subplot এ sine wave দেখাবে
```

### সব Axes এ Method Available

প্রতিটা axes এ সব method কাজ করে:

```python
# বিভিন্ন customization
axes[0].plot(x, y)
axes[0].set_xlabel('X axis')
axes[0].set_ylabel('Y axis')
axes[0].set_title('First Plot')

axes[1].plot(x, np.cos(x))
axes[1].set_title('Second Plot')

axes[2].plot(x, x**2)
axes[2].set_title('Third Plot')
```

---

## Different Subplot Configurations

### 2 Rows, 2 Columns

```python
fig, axes = plt.subplots(nrows=2, ncols=2, figsize=(10, 8))

# axes এখন 2D array
print(axes.shape)
# (2, 2)

# Indexing: axes[row][column]
axes[0, 0].plot(x, y)      # উপরের বামে
axes[0, 1].plot(x, y**2)   # উপরের ডানে
axes[1, 0].plot(x, y**3)   # নিচের বামে
axes[1, 1].plot(x, np.exp(x))  # নিচের ডানে

fig
```

### 3 Rows, 1 Column

```python
fig, axes = plt.subplots(nrows=3, ncols=1, figsize=(8, 12))

axes[0].plot(x, y)
axes[1].plot(x, y**2)
axes[2].plot(x, y**3)

fig
```

---

## Tight Layout

Overlapping text এবং labels এর সমস্যা solve করার জন্য `plt.tight_layout()` method ব্যবহার করা যায়।

### Without Tight Layout

```python
fig, axes = plt.subplots(nrows=1, ncols=3)

axes[0].plot(x, y)
axes[0].set_title('Plot 1')
axes[1].plot(x, y**2)
axes[1].set_title('Plot 2')
axes[2].plot(x, y**3)
axes[2].set_title('Plot 3')

fig
```

**Problem:**
- Text overlap করে
- দেখতে cluttered লাগে

### With Tight Layout

```python
fig, axes = plt.subplots(nrows=1, ncols=3)

axes[0].plot(x, y)
axes[0].set_title('Plot 1')
axes[1].plot(x, y**2)
axes[1].set_title('Plot 2')
axes[2].plot(x, y**3)
axes[2].set_title('Plot 3')

# Tight layout apply করা
plt.tight_layout()

fig
```

**Solution:**
- Automatically spacing adjust হয়
- Overlap থাকে না

**Best Practice:**
প্রথমে `figsize` দিয়ে proper size set করা উচিত, তারপর প্রয়োজনে `tight_layout()` ব্যবহার করা।

---

## Legend (লেজেন্ড) ব্যবহার

### Legend কেন দরকার?

একটা graph এ multiple plots থাকলে কোনটা কি বোঝা মুশকিল। Legend একটা label box যা প্রতিটা plot identify করতে সাহায্য করে।

### Basic Legend Example

```python
# Figure তৈরি
fig = plt.figure(figsize=(8, 8))

# Axes add করা
axes = fig.add_axes([0, 0, 1, 1])

# Multiple plots with labels
axes.plot(x, y, label='x, y')
axes.plot(x, x**2, label='x, x²')

# Legend show করা
axes.legend()

fig
```

**Output:**
```
Graph এর একটা corner এ label box দেখাবে যেখানে:
- নীল line = x, y
- কমলা line = x, x²
```

### কিভাবে কাজ করে?

1. **`label` parameter** - প্রতিটা plot এ label দিতে হয়
2. **`legend()` method** - Labels show করার জন্য call করতে হয়

```python
# Step 1: Plot with labels
axes.plot(x, y, label='Linear')
axes.plot(x, x**2, label='Quadratic')
axes.plot(x, x**3, label='Cubic')

# Step 2: Legend activate করা
axes.legend()
```

**Important:** `legend()` call না করলে label দিলেও show হবে না।

---

## Legend Location Control

### `loc` Parameter

Legend এর position control করার জন্য `loc` parameter ব্যবহার করা যায়।

```python
# Different locations
axes.legend(loc=0)  # Best location (automatic)
axes.legend(loc=1)  # Upper right
axes.legend(loc=2)  # Upper left
axes.legend(loc=3)  # Lower left
axes.legend(loc=4)  # Lower right
```

### Location Values

| Value | Position |
|-------|----------|
| 0 | Best (automatic - default) |
| 1 | Upper right |
| 2 | Upper left |
| 3 | Lower left |
| 4 | Lower right |

### Example with Location

```python
fig = plt.figure(figsize=(8, 6))
axes = fig.add_axes([0, 0, 1, 1])

axes.plot(x, y, label='sin(x)')
axes.plot(x, np.cos(x), label='cos(x)')

# Upper left corner এ legend
axes.legend(loc=2)

fig
```

**Default:** `loc=0` set করা থাকে যা automatically best position খুঁজে নেয়।

---

## Complete Example: Subplots with Legends

```python
# Data তৈরি
x = np.linspace(0, 10, 100)

# Figure তৈরি
fig, axes = plt.subplots(nrows=1, ncols=2, figsize=(12, 5))

# Left subplot
axes[0].plot(x, np.sin(x), label='sin(x)', color='blue')
axes[0].plot(x, np.cos(x), label='cos(x)', color='red')
axes[0].set_title('Trigonometric Functions')
axes[0].set_xlabel('X')
axes[0].set_ylabel('Y')
axes[0].legend(loc=1)
axes[0].grid(True)

# Right subplot
axes[1].plot(x, x, label='Linear', color='green')
axes[1].plot(x, x**2, label='Quadratic', color='orange')
axes[1].plot(x, x**3, label='Cubic', color='purple')
axes[1].set_title('Polynomial Functions')
axes[1].set_xlabel('X')
axes[1].set_ylabel('Y')
axes[1].legend(loc=2)
axes[1].grid(True)

# Layout adjust
plt.tight_layout()

fig
```

**Output:**
```
দুইটা professional subplot দেখাবে:
- বামে: sin এবং cos curves with legend
- ডানে: polynomial curves with legend
```

---

## Hidden Parameters এবং `**kwargs`

### `**kwargs` কি?

কিছু method এ `**kwargs` বা `**figure_kw` দেখা যায়। এর মানে হলো:

- Extra parameters pass করা যায়
- এই parameters আসলে internally অন্য method এর
- Documentation পড়ে বুঝতে হয়

### Example

```python
# subplots() method internally plt.figure() use করে
# তাই figure() এর parameters এখানে কাজ করে

plt.subplots(nrows=1, ncols=2, figsize=(10, 5))
#                                ↑
#                    এটা figure() এর parameter
```

**Documentation দেখা জরুরি** কারণ:
- এই hidden parameters বোঝার জন্য
- Proper usage শেখার জন্য
- Advanced features ব্যবহার করার জন্য

---

## Practical Tips

### 1. Figure Size সঠিকভাবে Set করা

```python
# ❌ ভুল - Default size
fig, axes = plt.subplots(nrows=2, ncols=3)
# Text overlap হবে

# ✅ সঠিক - Proper size
fig, axes = plt.subplots(nrows=2, ncols=3, figsize=(15, 10))
```

### 2. Tight Layout ব্যবহার

```python
# সবসময় শেষে tight_layout() add করা ভালো
plt.tight_layout()
```

### 3. Labels এবং Titles দেওয়া

```python
# প্রতিটা subplot এ proper labels
axes[0].set_xlabel('X axis')
axes[0].set_ylabel('Y axis')
axes[0].set_title('Plot Title')
```

### 4. Legend এর সঠিক ব্যবহার

```python
# Multiple plots থাকলে অবশ্যই legend
axes.plot(x, y1, label='Data 1')
axes.plot(x, y2, label='Data 2')
axes.legend()
```

---

## Common Mistakes

### ❌ Mistake 1: Legend call করা হয়নি

```python
axes.plot(x, y, label='My Data')
# legend() call করা হয়নি - label show হবে না
```

**✅ সঠিক:**
```python
axes.plot(x, y, label='My Data')
axes.legend()  # এটা লাগবে
```

### ❌ Mistake 2: Wrong Indexing

```python
fig, axes = plt.subplots(nrows=2, ncols=2)

# ভুল - single index
axes[0].plot(x, y)  # Error!

# সঠিক - 2D indexing
axes[0, 0].plot(x, y)
```

### ❌ Mistake 3: Size না দেওয়া

```python
# Multiple subplots এ size না দিলে দেখতে খারাপ লাগে
fig, axes = plt.subplots(nrows=3, ncols=3)
# Text overlap হবে
```

**✅ সঠিক:**
```python
fig, axes = plt.subplots(nrows=3, ncols=3, figsize=(12, 12))
```

---

## Key Takeaways

1. **`plt.subplots()`** - Figure এবং axes একসাথে তৈরি করার সবচেয়ে সহজ উপায়
2. **`nrows` এবং `ncols`** - Subplot layout define করার জন্য
3. **`figsize`** - Figure size control করার জন্য hidden parameter
4. **Indexing** - Multiple subplots এ আলাদা আলাদাভাবে plot করার জন্য
5. **`tight_layout()`** - Overlapping এড়ানোর জন্য
6. **`label` এবং `legend()`** - Multiple plots identify করার জন্য
7. **`loc` parameter** - Legend position control করার জন্য
8. **Documentation** - Advanced features এবং hidden parameters জানার জন্য

---

## Practice Tasks

1. ৩টা different mathematical functions একটা row এ plot করো
2. ২x২ grid এ ৪টা আলাদা plot তৈরি করো with proper labels
3. Legend location experiment করো (loc=0 থেকে 4)
4. `figsize` পরিবর্তন করে optimal size খুঁজে বের করো
5. একটা subplot এ ৩টা curves plot করো with legend

---

**Learning Approach:**
- Documentation পড়া শিখতে হবে
- নিজে experiment করতে হবে
- Parameters এর effect নিজে test করতে হবে

Happy Plotting! 📊