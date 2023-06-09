# Standard

## 1. Basic Style

### 1.1. String

- Prefer `f-string` to `.format()`

```python
# good
title = "Titanic"
print(f"Scatter Plot of = {title}")

# bad
print("Scatter Plot of {}".format(title))
```

### 1.2. Iteration

- Prefer `enumerate()` to `range(len())`

```python
xs = range(3)

# good
for ind, x in enumerate(xs):
  print(f'{ind}: {x}')

# bad
for i in range(len(xs)):
  print(f'{i}: {xs[i]}')
```

- Prefer `product` to nested `for` loops

```python
# good
from itertools import product

for item1, item2, item3 in product(list1, list2, list3):
    print(item1 + item2 + item3)

# bad
for item1 in list1:
    for item2 in list2:
        for item3 in list3:
              print(item1 + item2 + item3)
```

### 1.3. Condition

- Prefer `any()` or `all()` to `for...if`

```python
# good
found = any(thing == other_thing for thing in things)

# bad
found = False

for thing in things:
    if thing == other_thing:
        found = True
        break
```

## 2. Matplotlib Style

### 2.1. Objects

- Prefer `Axes` object to `Figure` object
- use `constrained_layout=True` when draw subplots

```python
# good
_, axes = plt.subplots(1, 2, figsize=(10, 6))
axes[0].plot(x1, y1)
axes[1].hist(x2, y2)

# bad
plt.subplot(121)
plt.plot(x1, y1)
plt.subplot(122)
plt.hist(x2, y2)
```

### 2.2. Subplots

- Prefer `axes.flatten()` to `plt.subplot()` in cases where subplots' data is iterable
- Prefer `zip()` or `enumerate()` to `range()` for iterable objects

```python
# good
_, ax = plt.subplots(2, 2)

for ax, x, y in zip(axes.flatten(), xs, ys):
  ax.plot(x, y)

# bad
for i in range(4):
  ax = plt.subplot(2, 2, i+1)
  ax.plot(x[i], y[i])
```

### 2.3. Decoration

- Prefer `set()` method to `set_*()` method when there is no need to tweak many parameters
- Prefer use `plt.setp()` to tweak parameters

```python
# good
ax.set(xlabel='x', ylabel='y')

# bad
ax.set_xlabel('x')
ax.set_ylabel('y')
```

- Prefer `ax.axis` to `ax.set(xlim, ylim)`

```python
# good
ax.axis([-1, 1, -1, 1])

# bad
ax.set(xlim=(-1, 1), ylim=(-1, 1))
```

- Prefer `ax.spines[loc_list]` to `ax.spines[loc]`

```python
# good
ax.spines[['right', 'top', 'left', 'bottom']].set_visible(False)
# bad
ax.spines['right'].set_visible(False)
ax.spines['top'].set_visible(False)
ax.spines['left'].set_visible(False)
ax.spines['bottom'].set_visible(False)
```

- Prefer `with plt.style.context(theme):` to `mpl.style.use(theme)` method when selecting theme for a single chart specifically

```python
# good
with plt.style.context(theme):
  plot_func()

# bad
mpl.style.use(theme)
```

- Prefer `@mpl.rc_context()` decorator to `plt.rcParams[]` method when tweaking styles of a single chart specifically

```python
# good
@mpl.rc_context({
    'font.family': 'sans-serif',
    'font.size': 9,
    'axes.titleweight': 'bold',
    'axes.titlesize': 12
})

# bad
plt.rcParams["font.family"] = "sans-serif"
plt.rcParams["font.size"] = 9
# plt.rcParams["axes.titleweight"] = "bold"
# plt.rcParams["axes.titlesize"] = 12
```

## 3. Pandas Style

- Prefer `df['col']` to `df.col`

```python
# good
movies['duration']

# bad
movies.duration
```

- Prefer `df.query` to `df[]` or `df.loc[]` in simple-selection

```python
# good
movies.query('duration >= 200')

# bad
movies[movies['duration'] >= 200]
movies.loc[movies['duration'] >= 200, :]
```

- Prefer `df.loc` and `df.iloc` to `df[]` in multiple-selection

```python
# good
movies.loc[movies['duration'] >= 200, 'genre']
movies.iloc[0:2, :]

# bad
movies[movies['duration'] >= 200].genre
movies[0:2]
```

- Prefer `.astype('category')` to `.factorize()`

```python

```

## 4. LaTeX Style

### 4.1. Multiple lines

Reduce the use of `begin{array}...end{array}`

- equations: `begin{aligned}...end{aligned}`

$$
\begin{aligned}
  y₁ &= x² + 2*x \\
  y₂ &= x³ + x
\end{aligned}
$$

```latex
$$
\begin{aligned}
  y₁ &= x² + 2*x \\
  y₂ &= x³ + x
\end{aligned}
$$
```

- equations with conditions: `begin{cases}...end{cases}`

$$
\begin{cases}
  y = x² + 2*x & x > 0 \\
  y = x³ + x & x ≤ 0
\end{cases}
$$

```latex
$$
\begin{cases}
  y = x² + 2*x & x > 0 \\
  y = x³ + x & x ≤ 0
\end{cases}
$$
```

- matrix: `begin{matrix}...end{matrix}`

$$
\begin{vmatrix}
  a + a^{′} & b + b^{′} \\ c & d
  \end{vmatrix} = \begin{vmatrix}
  a & b \\ c & d
  \end{vmatrix} + \begin{vmatrix}
  a^{′} & b^{′} \\ c & d
\end{vmatrix}
$$

```latex
$$
\begin{vmatrix}
  a + a^{′} & b + b^{′} \\ c & d
  \end{vmatrix} = \begin{vmatrix}
  a & b \\ c & d
  \end{vmatrix} + \begin{vmatrix}
  a^{′} & b^{′} \\ c & d
\end{vmatrix}
$$
```

### 4.2. Brackets

- Prefer `\bigg...\bigg` to `\left...\right`

$$
A\bigg[\frac{1}{2}\ \frac{1}{3}\ ⋯\ \frac{1}{99}\bigg]
$$

```latex
$$
A\bigg[\frac{1}{2}\ \frac{1}{3}\ ⋯\ \frac{1}{99}\bigg]
$$
```

- Prefer `\underset{}{}` to `\underset{}`

### 4.3. Expressions

- Prefer `^{\top}` to `^T` for transpose

$$
𝐀^{⊤}
$$

```latex
$$
𝐀^{\top}
$$
```

- Prefer `\to` to `\rightarrow` for limit

$$
\lim_{n → ∞}
$$

```latex
$$
\lim_{n\to \infty}
$$
```

- Prefer `underset{}{}` to `\limits_`

$$
\underset{w}{\mathrm{argmin}}(wx +b)
$$

```latex
$$
\underset{w}{\mathrm{argmin}}(wx +b)
$$
```

### 4.4. Fonts

- Prefer `mathrm{}` to `{\rm }` or `\mathop{}` or `\operatorname{}`

$$
θ_\mathrm{MLE} = \underset{θ}{\mathrm{argmax}}∑_{i = 1}^{N}\log p(x_{i} ∣ θ)
$$

```latex
$$
θ_\mathrm{MLE} = \underset{θ}{\mathrm{argmax}}∑_{i = 1}^{N}\log p(x_{i} ∣ θ)
$$
```

- Prefer `\mathbf{}` to `{\bf }`
- Prefer `\mathit{}` to `{\it }`

## 5. Reference

- [Hands-on Machine Learning with Scikit-Learn, Keras & TensorFlow](https://www.oreilly.com/library/view/hands-on-machine-learning/9781492032632/) (HandsOnML2, Machine Learning Sections)
- [Introduction to Probability](https://www.routledge.com/Introduction-to-Probability-Second-Edition/Blitzstein-Hwang/p/book/9781138369917) (ITP, Python Translation)
- [Statistical Rethinking](https://xcelab.net/rm/statistical-rethinking/) (SRT, Python Translation)
