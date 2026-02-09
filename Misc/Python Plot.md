```python
arr = np.array([3, 7, 2, 8, 5, 9, 1, 4, 6])

plt.figure(figsize=(10, 5))

# Method 1: Just plot the array (index is automatic)
plt.subplot(1, 2, 1)
plt.plot(arr, 'ro-')  # red circles with line
plt.title("Array Values vs Index")
plt.xlabel("Index")
plt.ylabel("Value")

# Method 2: With custom x-values
plt.subplot(1, 2, 2)
x_values = np.arange(len(arr))  # [0, 1, 2, ...]
plt.bar(x_values, arr)
plt.xticks(x_values)  # Show all indices
plt.title("Bar Plot of Array")
plt.xlabel("Index")
plt.ylabel("Value")

plt.tight_layout()
plt.show()
```

```python
import numpy as np
import matplotlib.pyplot as plt

# ───────────────────────────────────────
# 1. Simplest way – plot one array (y-values)
# ───────────────────────────────────────
y = np.array([3, 1, 4, 1, 5, 9, 2, 6, 5])
plt.plot(y)           # x = 0,1,2,...,n-1 automatically
plt.title("Just y values")
plt.grid(True, alpha=0.3)
plt.show()
```

```python
# ───────────────────────────────────────
# 2. Most common – plot x vs y (two 1D arrays)
# ───────────────────────────────────────
x = np.linspace(0, 10, 200)          # 200 points from 0 to 10
y = np.sin(x) * np.exp(-x/4)

plt.plot(x, y, color='teal', linewidth=2, label='damped sine')
plt.xlabel('Time (s)')
plt.ylabel('Amplitude')
plt.title('Damped Sine Wave')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

```python
# ───────────────────────────────────────
# 3. Multiple lines on same plot
# ───────────────────────────────────────
t = np.linspace(0, 5, 300)

plt.plot(t, np.sin(t), label='sin(t)')
plt.plot(t, np.cos(t), label='cos(t)')
plt.plot(t, np.sin(t)*np.cos(t), '--', label='sin·cos', alpha=0.7)

plt.legend(loc='upper right')
plt.title('Several functions')
plt.grid(ls='--', alpha=0.4)
plt.show()
```


```python
# ───────────────────────────────────────
# 4. Plotting columns of a 2D array (very common!)
# ───────────────────────────────────────
data = np.random.randn(100, 4).cumsum(axis=0)   # shape (100,4)

# Option A – automatic (one line per column)
plt.plot(data, lw=1.2)
plt.title("4 random walks (each column = one line)")
plt.show()

# Option B – more control
plt.figure(figsize=(9,5))
for i in range(data.shape[1]):
    plt.plot(data[:, i], label=f'Series {i+1}', alpha=0.9)
plt.legend()
plt.title("Same data – explicit loop")
plt.grid(True, alpha=0.3)
plt.show()
```

```python
# ───────────────────────────────────────
# 5. Quick scatter / points
# ───────────────────────────────────────
x = np.random.uniform(-3, 3, 500)
y = np.random.normal(0, 1, 500) + x**2

plt.scatter(x, y, s=8, alpha=0.6, c=y, cmap='viridis')
plt.colorbar(label='y value')
plt.title("Scatter from two arrays")
plt.show()
```


---
### 1. Same plot – most common for comparison

```python
import numpy as np
import matplotlib.pyplot as plt

# Example data
x = np.linspace(0, 10, 300)
y1 = np.sin(x) + 0.3 * np.random.randn(len(x))          # noisy sine
y2 = np.sin(x + 0.8)                                    # phase-shifted sine

# ───────────────────────────────────────────────
# Style A – simple & clean
# ───────────────────────────────────────────────
plt.figure(figsize=(10, 5.5))

plt.plot(x, y1, label='signal A (noisy)', lw=1.8, alpha=0.9)
plt.plot(x, y2, label='signal B', lw=2.2, color='darkorange')

plt.title("Direct comparison – same axes")
plt.xlabel("Time")
plt.ylabel("Value")
plt.legend(frameon=True, fancybox=True, shadow=True)
plt.grid(True, alpha=0.35, ls='--')
plt.tight_layout()
plt.show()
```

### 2. Same plot – when you want to emphasize difference

```python
# ───────────────────────────────────────────────
# Style B – plot both + difference (very readable)
# ───────────────────────────────────────────────
plt.figure(figsize=(11, 6))

plt.plot(x, y1, label='A', lw=1.4, alpha=0.85)
plt.plot(x, y2, label='B', lw=1.4, alpha=0.85)
plt.fill_between(x, y1, y2, color='purple', alpha=0.12, label='difference')

plt.title("A vs B + shaded difference region")
plt.legend()
plt.grid(alpha=0.3)
plt.show()
```

### 3. Two separate subplots (different axes)

Very useful when:

- ranges are very different
- you want different y-scales
- you want to apply different styling/zoom per signal

```python
# ───────────────────────────────────────────────
# Two subplots – shared x-axis (most popular)
# ───────────────────────────────────────────────
fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(10, 7), sharex=True)

ax1.plot(x, y1, color='#1f77b4', lw=1.8, label='signal A')
ax1.set_title('Signal A')
ax1.grid(alpha=0.3)
ax1.legend(loc='upper right')

ax2.plot(x, y2, color='#ff7f0e', lw=1.8, label='signal B')
ax2.set_title('Signal B')
ax2.grid(alpha=0.3)
ax2.legend(loc='upper right')
ax2.set_xlabel('Time')

fig.suptitle("Comparison – separate y-axes", fontsize=14, y=0.98)
plt.tight_layout(rect=[0, 0, 1, 0.96])   # make room for suptitle
plt.show()
```

### 4. Side-by-side (twin axes style – one figure)

Good when you want to keep the same x-scale but different y-scales.

```python
fig, ax1 = plt.subplots(figsize=(10, 5))

ax1.plot(x, y1, color='C0', lw=2, label='A (left axis)')
ax1.set_ylabel('A amplitude', color='C0')
ax1.tick_params(axis='y', labelcolor='C0')
ax1.grid(alpha=0.25)

ax2 = ax1.twinx()                    # ← key line
ax2.plot(x, y2 * 5, color='C3', lw=2, label='B (right axis ×5)')
ax2.set_ylabel('B amplitude (×5)', color='C3')
ax2.tick_params(axis='y', labelcolor='C3')

fig.suptitle("Comparison with different scales (twin axes)")
fig.legend(loc='upper center', ncol=2, bbox_to_anchor=(0.5, 0.92))
plt.tight_layout()
plt.show()
```

### Quick decision table

| Goal / Situation                              | Recommended approach                  | Code pattern to use          |
|-----------------------------------------------|---------------------------------------|-------------------------------|
| Same range, want to see overlap/delay         | Same plot                             | `plt.plot()` twice            |
| Want to clearly see the difference            | Same plot + `fill_between`            | difference shading            |
| Very different amplitudes / units             | Two subplots (sharex=True)            | `plt.subplots(2,1)`           |
| Same x, but want independent y-scales         | Twin axes (`twinx()`)                 | `ax.twinx()`                  |
| Many signals + comparison                     | Same plot with good alpha/lw          | multiple `plot()` + legend    |
| Need exact numerical comparison               | Same plot + zoom + cursor readout*    | interactive mode              |



### 1. Positional arguments (the data — order matters!)

| Position | Name       | Type              | What it does                                                                 | Default / Notes                                 |
|----------|------------|-------------------|-------------------------------------------------------------------------------|-------------------------------------------------|
| 1st      | `x`        | array-like or None| x-coordinates                                                                | If missing → uses `range(len(y))` i.e. 0,1,2,… |
| 2nd      | `y`        | array-like        | y-coordinates (almost always required)                                       | —                                               |
| 3rd      | `fmt`      | str               | Shortcut for **color + marker + linestyle** (very popular)                  | '' (no shortcut)                                |

**fmt** examples (you combine 1 char from each group):

- `'b-'`   → blue solid line  
- `'r--'`  → red dashed line  
- `'go'`   → green circles (no line)  
- `'k^:'`  → black triangles + dotted line  
- `'c*-'`  → cyan stars + solid line  

Full order inside fmt: `[marker][line][color]` (you can skip any part)

### 2. Most useful keyword arguments (the ones you change most often)

| Parameter          | Meaning / What it controls                              | Common values / examples                              | Default          |
|---------------------|----------------------------------------------------------------|----------------------------------------------------------|------------------|
| `color` / `c`       | Line / marker color                                            | `'blue'`, `'#FF4500'`, `'0.4'` (gray), `'C0'`…`'C9'` (cycle) | from color cycle |
| `linewidth` / `lw`  | Thickness of the line                                          | `0.8`, `1.5`, `3`, `0.3`                                 | `1.5`            |
| `linestyle` / `ls`  | Line style                                                     | `'-'` solid, `'--'` dashed, `':'` dotted, `'-.'` dash-dot, `''` / `'none'` no line | `'-'`            |
| `marker`            | Shape of points                                                | `'o'`, `'.'`, `'s'` square, `'^'` triangle, `'x'`, `'+'`, `'*'`, `'D'` diamond | `None` (no markers) |
| `markersize` / `ms` | Size of markers                                                | `4`, `6`, `8`, `12`                                      | `6`              |
| `markeredgecolor` / `mec` | Border color of markers                                 | `'k'`, `'white'`, `None`                                 | same as `color`  |
| `markerfacecolor` / `mfc` | Fill color of markers                                   | `'none'`, `'w'`, `'#ffcc00'`                             | same as `color`  |
| `alpha`             | Transparency (0 = invisible, 1 = opaque)                       | `0.3`, `0.6`, `0.9`                                      | `1.0`            |
| `label`             | Text shown in legend                                       | `'measured'`, `'model v3'`, `'data 2025'`                | `None` (no legend entry) |
| `zorder`            | Drawing order (higher = drawn later / on top)                  | `1`, `2.5`, `10`                                         | `2`              |

### Quick examples combining them

```python
# Very thin light dashed line with big red markers
plt.plot(x, y, color='darkred', lw=0.8, ls='--', marker='o', ms=9, mfc='none', mec='darkred', alpha=0.7)

# Clean modern look – thick line, no markers, semi-transparent
plt.plot(x, y, c='teal', lw=3.2, alpha=0.85, label='simulation')

# Just points, no connecting line
plt.plot(x, y, 'k.', ms=3.5, alpha=0.6)          # black dots, small, a bit transparent

# Star markers with thick edge, filled yellow
plt.plot(x, y, marker='*', ms=14, mfc='gold', mec='navy', mew=1.8, linestyle='')
```

### 3. Less frequent but very useful parameters

| Parameter     | Purpose                                                                 | Common values                              |
|---------------|--------------------------------------------------------------------------|--------------------------------------------|
| `drawstyle`   | How the line connects points (mostly for step-like data)                | `'default'`, `'steps'`, `'steps-pre'`, `'steps-mid'`, `'steps-post'` |
| `dash_capstyle` | How dashes end (sharp vs round)                                       | `'butt'`, `'round'`, `'projecting'`        |
| `solid_capstyle` | Same but for solid lines                                             | `'butt'`, `'round'`, `'projecting'`        |
| `antialiased` | Smooth jagged lines (usually good)                                      | `True` / `False`                           |
| `clip_on`     | Whether points outside axis limits are drawn                            | `True` (default) / `False`                 |
| `scalex` / `scaley` | Whether this line should update the axis limits automatically     | `True` / `False`                           |

### 4. Pattern you will use 95% of the time

```python
plt.plot(
    x, y,                     # data
    'b-',                     # or skip and use keywords below
    color='royalblue',
    linewidth=1.9,
    linestyle='-',            # or '--', ':', '-.'
    marker='o',
    markersize=5,
    markerfacecolor='white',
    markeredgecolor='royalblue',
    markeredgewidth=1.4,
    alpha=0.9,
    label='Experiment A',
    zorder=3
)
```
