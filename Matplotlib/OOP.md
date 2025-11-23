# Matplotlib - Object-Oriented Approach & Customization

## Two Approaches in Matplotlib

Matplotlib এ দুটো approach আছে plotting এর জন্য:

| Approach | Description | Use Case |
|----------|-------------|----------|
| **Functional** | `plt.plot()`, `plt.xlabel()` | Quick plots |
| **Object-Oriented** | Figure & Axes objects | Advanced customization |

**এই tutorial এ:** Object-Oriented approach শিখবো।

---

## Line Customization (Quick Review)

### Dashed Lines

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(5, 20)
y = x * 5 + 1

# Dashed line
plt.plot(x, y, 'r--')  # Red dashed
plt.show()
```

### Markers

```python
# Plus markers
plt.plot(x, y, 'r+-')  # Red, plus markers, dashed line
plt.show()
```

**Format String:** `'[color][marker][line]'`
- Color: `r`, `g`, `b`, `k`, etc.
- Marker: `+`, `o`, `*`, `s`, etc.
- Line: `-`, `--`, `:`, `-.`

---

## Object-Oriented Approach

### Why OOP Approach?

**Functional approach limitations:**
- Limited customization
- Hard to modify after creation
- Fixed structure

**OOP approach benefits:**
- ✅ Full control over everything
- ✅ Can modify anytime
- ✅ Multiple plots easily managed
- ✅ Professional-level customization

---

## Figure Object

### Creating a Figure

```python
import matplotlib.pyplot as plt
import numpy as np

# Data
x = np.arange(5, 20)
y = x * 5 + 1

# Create Figure object
fig = plt.figure()
print(fig)
```

**Output:**
```
Figure(640x480)
```

**Figure** = Empty canvas for plotting

---

## Axes Object

### Adding Axes to Figure

Axes = Plot area with specific dimensions

```python
# Create figure
fig = plt.figure()

# Add axes
axes = fig.add_axes([0.1, 0.1, 0.8, 0.8])

print(axes)
```

**Output:**
```
<AxesSubplot:>
```

### add_axes() Parameters

```python
fig.add_axes([left, bottom, width, height])
```

**All values between 0 and 1 (fractions):**
- `left` - কতটা left থেকে শুরু (0 = extreme left)
- `bottom` - কতটা bottom থেকে শুরু (0 = extreme bottom)
- `width` - কতটা wide (1 = full width)
- `height` - কতটা tall (1 = full height)

**Example:**
```python
[0.1, 0.1, 0.8, 0.8]
```
- Start: 10% from left, 10% from bottom
- Size: 80% width, 80% height

---

## Complete Example

### Plotting with Axes

```python
# Create figure
fig = plt.figure()

# Add axes
axes = fig.add_axes([0.1, 0.1, 0.8, 0.8])

# Plot on axes
axes.plot(x, y)

# Display
fig
```

**Perfect!** Plot দেখাচ্ছে।

---

## Multiple Axes (Plot in Plot)

### Nested Plots

```python
# Create figure
fig = plt.figure()

# Main axes (outer plot)
axes1 = fig.add_axes([0.1, 0.1, 0.8, 0.8])
axes1.plot(x, y, 'r')

# Inner axes (smaller plot)
axes2 = fig.add_axes([0.2, 0.5, 0.3, 0.3])
axes2.plot(x, y)

# Display
fig
```

**Output:**

বড় plot এর ভেতরে একটা ছোট plot! 🎨

### Position Explained

**Main plot:**
- `[0.1, 0.1, 0.8, 0.8]` → Standard size

**Inner plot:**
- `[0.2, 0.5, 0.3, 0.3]` → Smaller, top-left corner

---

## Common Mistake ⚠️

### DON'T Re-run add_axes()

```python
# ❌ Wrong - creates duplicate axes
fig.add_axes([0.1, 0.1, 0.8, 0.8])
fig.add_axes([0.1, 0.1, 0.8, 0.8])  # Another axes!
```

**Problem:** `.add_axes()` adds new axes, doesn't replace old ones.

### ✅ Solution: Use set_position()

```python
# Create axes once
axes2 = fig.add_axes([0.2, 0.5, 0.3, 0.3])

# Later, change position
axes2.set_position([0.4, 0.6, 0.3, 0.3])
```

---

## Modifying Axes Position

### set_position() Method

```python
# Create figure
fig = plt.figure()

# Main plot
axes1 = fig.add_axes([0.1, 0.1, 0.8, 0.8])
axes1.plot(x, y, 'r')

# Inner plot
axes2 = fig.add_axes([0.2, 0.5, 0.3, 0.3])
axes2.plot(x, y)

# Change inner plot position
axes2.set_position([0.2, 0.5, 0.3, 0.3])

fig
```

**Use case:** Adjust plot positions after creation

---

## Adding Labels & Titles

### OOP Syntax

```python
# Create figure and axes
fig = plt.figure()
axes = fig.add_axes([0.1, 0.1, 0.8, 0.8])

# Plot
axes.plot(x, y)

# Add labels (OOP style)
axes.set_xlabel('X Axis')
axes.set_ylabel('Y Axis')
axes.set_title('Title')

fig
```

**Notice:**
- `plt.xlabel()` → `axes.set_xlabel()`
- `plt.ylabel()` → `axes.set_ylabel()`
- `plt.title()` → `axes.set_title()`

---

## Functional vs OOP Comparison

### Functional Approach

```python
# Quick and simple
plt.plot(x, y)
plt.xlabel('X')
plt.ylabel('Y')
plt.title('My Plot')
plt.show()
```

### OOP Approach

```python
# More control
fig = plt.figure()
axes = fig.add_axes([0.1, 0.1, 0.8, 0.8])
axes.plot(x, y)
axes.set_xlabel('X')
axes.set_ylabel('Y')
axes.set_title('My Plot')
fig
```

**কখন কোনটা?**
- **Functional:** Quick exploration, simple plots
- **OOP:** Professional work, complex layouts, customization

---

## Complete Example: Main + Inset Plot

```python
import numpy as np
import matplotlib.pyplot as plt

# Data
x = np.arange(5, 20)
y = x * 5 + 1

# Create figure
fig = plt.figure()

# Main plot (large)
axes1 = fig.add_axes([0.1, 0.1, 0.8, 0.8])
axes1.plot(x, y, 'r')
axes1.set_xlabel('X Axis')
axes1.set_ylabel('Y Axis')
axes1.set_title('Main Plot')

# Inset plot (small, top-right)
axes2 = fig.add_axes([0.5, 0.5, 0.3, 0.3])
axes2.plot(y, x, 'b')
axes2.set_xlabel('Y Values')
axes2.set_ylabel('X Values')
axes2.set_title('Inset')

fig
```

---

## Position Examples

### Bottom-left Inset

```python
axes2 = fig.add_axes([0.15, 0.15, 0.25, 0.25])
```

### Top-right Inset

```python
axes2 = fig.add_axes([0.6, 0.6, 0.25, 0.25])
```

### Center Inset

```python
axes2 = fig.add_axes([0.4, 0.4, 0.3, 0.3])
```

### Side-by-side

```python
# Left plot
axes1 = fig.add_axes([0.1, 0.1, 0.35, 0.8])

# Right plot
axes2 = fig.add_axes([0.55, 0.1, 0.35, 0.8])
```

---

## Key Methods Summary

### Figure Methods

| Method | কি করে |
|--------|--------|
| `plt.figure()` | Empty figure তৈরি |
| `fig.add_axes()` | Axes add করা |

### Axes Methods

| Method | কি করে |
|--------|--------|
| `axes.plot()` | Line plot |
| `axes.set_xlabel()` | X axis label |
| `axes.set_ylabel()` | Y axis label |
| `axes.set_title()` | Plot title |
| `axes.set_position()` | Position change |

---

## Practice Task

তোমার জন্য task:

```python
# এই code complete করো:

fig = plt.figure()

# Main plot
axes1 = fig.add_axes([0.1, 0.1, 0.8, 0.8])
axes1.plot(x, y, 'g--')
axes1.set_xlabel('X Values')
axes1.set_ylabel('Y Values')
axes1.set_title('Green Dashed Line')

# Inner plot (তুমি position দাও)
axes2 = fig.add_axes([?, ?, ?, ?])
axes2.plot(y, x, 'r+')
# এখানে labels এবং title দাও
axes2.set_xlabel('?')
axes2.set_ylabel('?')
axes2.set_title('?')

fig
```

---

## Best Practices

### ✅ DO

1. **OOP approach** complex plots এর জন্য
2. **Meaningful names** axes variables এর
3. **set_position()** position change করতে
4. **Labels always** দাও (readability)
5. **Comments** লিখো position values এর

### ❌ DON'T

1. **Re-run add_axes()** - duplicates তৈরি হয়
2. **Skip labels** - confusing হয়
3. **Too many insets** - cluttered দেখায়
4. **Random positions** - plan করো আগে

---

## Common Patterns

### Pattern 1: Single Plot

```python
fig = plt.figure()
axes = fig.add_axes([0.1, 0.1, 0.8, 0.8])
axes.plot(x, y)
axes.set_xlabel('X')
axes.set_ylabel('Y')
fig
```

### Pattern 2: Plot with Inset

```python
fig = plt.figure()
axes1 = fig.add_axes([0.1, 0.1, 0.8, 0.8])
axes2 = fig.add_axes([0.5, 0.5, 0.3, 0.3])
axes1.plot(x, y)
axes2.plot(y, x)
fig
```

### Pattern 3: Modify After Creation

```python
fig = plt.figure()
axes = fig.add_axes([0.1, 0.1, 0.8, 0.8])
axes.plot(x, y)
axes.set_position([0.15, 0.15, 0.7, 0.7])  # Adjust
fig
```

---

## Why Learn OOP Approach?

### Seaborn Connection

Seaborn (পরে শিখবো) Matplotlib এর উপর built। Seaborn internally Figure এবং Axes objects use করে।

**যদি Figure/Axes বুঝো:**
- Seaborn plots customize করতে পারবে
- Matplotlib এর power + Seaborn এর simplicity
- Professional-quality plots তৈরি করতে পারবে

### Real-World Use

Professional data visualization এ:
- Multiple subplots একসাথে
- Custom layouts (publications এর জন্য)
- Fine-tuned positioning
- Interactive dashboards

**OOP approach = Full control!**

---

## Quick Reference

```python
# Create figure
fig = plt.figure()

# Add axes
axes = fig.add_axes([left, bottom, width, height])

# Plot
axes.plot(x, y, 'style')

# Labels
axes.set_xlabel('X Label')
axes.set_ylabel('Y Label')
axes.set_title('Title')

# Modify position
axes.set_position([new_left, new_bottom, new_width, new_height])

# Display
fig
```

---

## Key Takeaways

1. **Two approaches:** Functional (simple) vs OOP (powerful)
2. **Figure** = Canvas for plotting
3. **Axes** = Individual plot area
4. **add_axes([l, b, w, h])** = Position এবং size
5. All values **0 to 1** (fractions)
6. **set_position()** = Modify after creation
7. **DON'T re-run** add_axes() - duplicates তৈরি হয়
8. **OOP approach** = Professional customization
9. **Seaborn** internally এটা use করে
10. Practice করতে থাকো! 💪

---

## Next Steps

এখন যা জানো:
- Functional approach basics
- OOP approach with Figure & Axes
- Position customization
- Multiple plots in one figure
- Labels & titles (both approaches)

পরবর্তীতে শিখবো:
- **Seaborn** (easier, prettier plots)
- Different plot types (scatter, bar, histogram)
- Styling এবং themes
- Real data visualization

Happy Plotting! 📊🎨