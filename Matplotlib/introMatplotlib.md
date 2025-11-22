# Matplotlib - Introduction to Data Visualization

## Matplotlib কি?

**Matplotlib** হলো Python এর সবচেয়ে powerful এবং customizable data visualization library।

**Key Points:**
- Mathematical plotting library
- MATLAB এর মতো syntax
- NumPy দিয়ে তৈরি
- সবচেয়ে flexible plotting tool

**Name Origin:** MAT (Mathematical) + PLOT (Plotting) + LIB (Library)

---

## Installation

```bash
# Conda দিয়ে
conda install matplotlib

# Pip দিয়ে
pip install matplotlib
```

---

## Basic Setup

### Import করা

```python
import numpy as np
import matplotlib.pyplot as plt
```

**Convention:**
- `matplotlib.pyplot` → `plt` (standard alias)
- এটা plotting এর main module

---

## প্রথম Plot তৈরি করা

### Data তৈরি

```python
# X axis data
x = np.arange(5, 20)

# Y axis data (x এর সাথে connected)
y = x * 5 + 1

print("X:", x)
print("Y:", y)
```

**Output:**
```
X: [ 5  6  7  8  9 10 11 12 13 14 15 16 17 18 19]
Y: [ 26  31  36  41  46  51  56  61  66  71  76  81  86  91  96]
```

### Simple Line Plot

```python
# Plot তৈরি
plt.plot(x, y)
plt.show()
```

**Output:**

একটা straight line graph - X axis এ 5-19, Y axis এ 26-96!

---

## Plot Customization

### Adding Labels এবং Title

```python
# Data plot
plt.plot(x, y)

# X axis label
plt.xlabel('X Axis')

# Y axis label
plt.ylabel('Y Axis')

# Title
plt.title('Title')

# Display
plt.show()
```

**এখন graph informative!**

### Color Change করা

```python
# Red color line
plt.plot(x, y, 'r')  # 'r' = red

plt.xlabel('X Axis')
plt.ylabel('Y Axis')
plt.title('Title')
plt.show()
```

**Common Colors:**
- `'r'` → Red
- `'g'` → Green
- `'b'` → Blue
- `'y'` → Yellow
- `'k'` → Black
- `'m'` → Magenta
- `'c'` → Cyan

---

## plt কিভাবে কাজ করে?

### Global State Machine

```python
plt.plot(x, y)
plt.xlabel('X Axis')
plt.ylabel('Y Axis')
plt.title('My Graph')
plt.show()
```

**Magic:**
- `plt.show()` এর আগে সব methods একই graph এ apply হয়
- `plt` একটা global state maintain করে
- একবার `.show()` call করলে graph complete

### Multiple Graphs?

```python
# First graph
plt.plot(x, y)
plt.show()

# Second graph (নতুন graph শুরু)
plt.plot(y, x)
plt.show()
```

`.show()` একটা endpoint - নতুন graph শুরু করে!

---

## Subplots - একসাথে Multiple Plots

একাধিক plots পাশাপাশি দেখাতে।

### Basic Subplots

```python
# 1 row, 2 columns layout
# Plot 1 (left)
plt.subplot(1, 2, 1)
plt.plot(x, y, 'r')

# Plot 2 (right)
plt.subplot(1, 2, 2)
plt.plot(y, x)

plt.show()
```

**Output:**

দুইটা plots পাশাপাশি!

### subplot() Syntax

```python
plt.subplot(rows, cols, plot_number)
```

**Parameters:**
- `rows` - কতগুলো rows
- `cols` - কতগুলো columns  
- `plot_number` - কোন position এ (left to right)

**Examples:**
```python
# 1 row, 2 columns, position 1
plt.subplot(1, 2, 1)

# 2 rows, 1 column, position 2
plt.subplot(2, 1, 2)

# 2 rows, 2 columns, position 3
plt.subplot(2, 2, 3)
```

---

## Complete Example

### Side-by-Side Plots

```python
import numpy as np
import matplotlib.pyplot as plt

# Data
x = np.arange(5, 20)
y = x * 5 + 1

# Plot 1: Normal
plt.subplot(1, 2, 1)
plt.plot(x, y, 'r')
plt.xlabel('X Values')
plt.ylabel('Y Values')
plt.title('Normal Plot')

# Plot 2: Reversed
plt.subplot(1, 2, 2)
plt.plot(y, x, 'b')
plt.xlabel('Y Values')
plt.ylabel('X Values')
plt.title('Reversed Plot')

plt.show()
```

**Output:**

- **Left:** Red line (X vs Y)
- **Right:** Blue line (Y vs X)

---

## Grid Layout Examples

### 2x2 Grid

```python
# 2 rows, 2 columns = 4 plots
plt.subplot(2, 2, 1)  # Top-left
plt.plot(x, y)

plt.subplot(2, 2, 2)  # Top-right
plt.plot(y, x)

plt.subplot(2, 2, 3)  # Bottom-left
plt.plot(x, x**2)

plt.subplot(2, 2, 4)  # Bottom-right
plt.plot(x, np.sqrt(x))

plt.show()
```

### Vertical Stack

```python
# 3 rows, 1 column
plt.subplot(3, 1, 1)
plt.plot(x, y)

plt.subplot(3, 1, 2)
plt.plot(x, y**2)

plt.subplot(3, 1, 3)
plt.plot(x, np.log(y))

plt.show()
```

---

## Plot Types Overview

Matplotlib এ অনেক ধরনের plots আছে:

| Plot Type | Use Case |
|-----------|----------|
| Line Plot | Continuous data trends |
| Scatter Plot | Data point distribution |
| Bar Chart | Categorical comparisons |
| Histogram | Data distribution |
| Pie Chart | Proportions/percentages |
| Box Plot | Statistical summary |
| Heatmap | Matrix data |

---

## Key Methods Summary

| Method | কি করে |
|--------|--------|
| `plt.plot()` | Line plot তৈরি |
| `plt.xlabel()` | X axis label |
| `plt.ylabel()` | Y axis label |
| `plt.title()` | Graph title |
| `plt.show()` | Display graph |
| `plt.subplot()` | Multiple plots |

---

## Best Practices

### ✅ DO

1. সবসময় **labels** এবং **title** দাও
2. Appropriate **colors** use করো
3. `.show()` দিয়ে graph close করো
4. Multiple plots এর জন্য **subplots** use করো

### ❌ DON'T

1. `.show()` ছাড়া next graph start করো না
2. Labels ছাড়া plot দিও না
3. অনেক plots একসাথে clutter করো না

---

## Common Patterns

### Pattern 1: Basic Plot

```python
plt.plot(x, y)
plt.xlabel('X')
plt.ylabel('Y')
plt.title('My Plot')
plt.show()
```

### Pattern 2: Colored Plot

```python
plt.plot(x, y, 'r')  # Red line
plt.xlabel('X')
plt.ylabel('Y')
plt.show()
```

### Pattern 3: Subplots

```python
plt.subplot(1, 2, 1)
plt.plot(x, y)

plt.subplot(1, 2, 2)
plt.plot(y, x)

plt.show()
```

---

## Matplotlib vs Other Libraries

### Matplotlib
- ✅ সবচেয়ে powerful
- ✅ Maximum customization
- ❌ Complex syntax
- ❌ More code লাগে

### Seaborn (পরে শিখবো)
- ✅ Built on Matplotlib
- ✅ Easier syntax
- ✅ Better default styles
- ✅ Statistical plots
- ❌ Less customization

**Pandas NumPy এর মতো:**
- NumPy → Complete কিন্তু complex
- Pandas → NumPy এর উপর built, easier
- Matplotlib → Complete কিন্তু complex
- Seaborn → Matplotlib এর উপর built, easier

---

## Documentation & Resources

### Official Website
https://matplotlib.org

**Sections:**
- **Plot Types** - সব ধরনের plots
- **Examples** - Working code
- **Tutorials** - Step-by-step guides
- **Cheat Sheet** - Quick reference

### Learning Path

1. ✅ এই tutorial (basics)
2. Official examples দেখো
3. Different plot types practice করো
4. Seaborn শিখো (easier)
5. Complex projects এ Matplotlib use করো

---

## Quick Reference

```python
# Import
import matplotlib.pyplot as plt
import numpy as np

# Data
x = np.arange(0, 10)
y = x ** 2

# Basic plot
plt.plot(x, y)
plt.show()

# Customized plot
plt.plot(x, y, 'r')
plt.xlabel('X axis')
plt.ylabel('Y axis')
plt.title('My Graph')
plt.show()

# Subplots
plt.subplot(1, 2, 1)
plt.plot(x, y)

plt.subplot(1, 2, 2)
plt.plot(y, x)

plt.show()
```

---

## Key Takeaways

1. **Matplotlib** = Most powerful visualization library
2. **`plt.plot()`** = Basic line plot
3. **Labels** always দাও (xlabel, ylabel, title)
4. **Colors** = single letter ('r', 'g', 'b')
5. **`.show()`** = Graph display এবং close
6. **`subplot()`** = Multiple plots একসাথে
7. Global **state machine** - সব methods একই graph এ
8. **Seaborn** আরো easy (পরে শিখবো)

---

## Next Steps

এখন যা জানো:
- Matplotlib basics
- Simple line plots
- Labels এবং titles
- Colors
- Subplots

পরবর্তীতে শিখবো:
- Different plot types
- Advanced customization
- Seaborn library
- Real data visualization

Happy Plotting! 📊🚀