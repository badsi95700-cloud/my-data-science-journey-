# Data Visualization Complete Guide

Comprehensive guide to Matplotlib, Seaborn, and Plotly for creating beautiful and informative visualizations.

## ML toolbox curriculum map (this guide)

Short map for the **visualization** part of the ML toolbox (all with plotting code below).

- **Matplotlib: line, scatter, histogram, bar & pie** → [Common plot types](#common-plot-types)
- **Seaborn: statistical & categorical plots** → [Seaborn sections](#seaborn-introduction)
- **Plotly: interactive visualizations** → [Plotly and Dash](#plotly-and-dash-for-interactive-visualizations)
- **Underlying data prep** → [NumPy](01-numpy.md) and [Pandas](02-pandas.md) guides

## Table of Contents

- [ML toolbox curriculum map (this guide)](#ml-toolbox-curriculum-map-this-guide)
- [Introduction](#introduction)
- [Matplotlib Basics](#matplotlib-basics)
- [Figure and Axes Structure](#figure-and-axes-structure)
- [Common Plot Types](#common-plot-types)
- [Customizing Plots](#customizing-plots)
- [Subplots and Multiple Plots](#subplots-and-multiple-plots)
- [Seaborn Introduction](#seaborn-introduction)
- [Seaborn Relational Plots](#seaborn-relational-plots)
- [Seaborn Categorical Plots](#seaborn-categorical-plots)
- [Seaborn Distribution Plots](#seaborn-distribution-plots)
- [Seaborn Regression & Mixed Plots](#seaborn-regression--mixed-plots)
- [Seaborn Matrix & Styling Plots](#seaborn-matrix--styling-plots)
- [Plotly and Dash for Interactive Visualizations](#plotly-and-dash-for-interactive-visualizations)
- [Saving Figures](#saving-figures)
- [Key Takeaways](#key-takeaways)
- [Common Patterns](#common-patterns)
- [Practice Exercises](#practice-exercises)

---

## Introduction

### What is Matplotlib?

Matplotlib is Python's primary plotting library. It provides:

- **Flexibility**: Create any type of plot
- **Control**: Fine-grained control over every element
- **Publication quality**: High-quality figures for papers

### What is Seaborn?

Seaborn is built on Matplotlib and provides:

- **Statistical plots**: Built-in statistical visualizations
- **Beautiful defaults**: Attractive styling out of the box
- **Easy to use**: Less code for common plots
- **Integration**: Works seamlessly with Pandas DataFrames

### When to Use What?

| Library | Best For |
|---------|----------|
| **Matplotlib** | Custom plots, full control, publication figures |
| **Seaborn** | Statistical plots, quick EDA, attractive defaults |
| **Plotly** | Interactive plots, dashboards, web apps |

### Installation

```python
pip install matplotlib seaborn plotly dash
```

```python
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
import pandas as pd

# Set style
plt.style.use('seaborn-v0_8')  # or 'default', 'ggplot', etc.
sns.set_style("whitegrid")
```

---

## Matplotlib Basics

### Simple Plot

```python
# Create data
x = np.linspace(0, 10, 100)
y = np.sin(x)

# Create plot
plt.figure(figsize=(10, 6))
plt.plot(x, y)
plt.xlabel('X axis')
plt.ylabel('Y axis')
plt.title('Simple Sine Wave')
plt.grid(True)
plt.show()
```

### Multiple Lines

```python
x = np.linspace(0, 10, 100)
y1 = np.sin(x)
y2 = np.cos(x)

plt.figure(figsize=(10, 6))
plt.plot(x, y1, label='sin(x)')
plt.plot(x, y2, label='cos(x)')
plt.xlabel('X')
plt.ylabel('Y')
plt.title('Sine and Cosine')
plt.legend()
plt.grid(True)
plt.show()
```

### Figure and Axes

```python
# Explicit figure and axes — preferred approach
fig, ax = plt.subplots(figsize=(10, 6))
ax.plot(x, y)
ax.set_xlabel('X axis')
ax.set_ylabel('Y axis')
ax.set_title('Using Axes Object')
ax.grid(True)
plt.show()
```

> 💡 **Tip:** Always use `fig, ax = plt.subplots()` over `plt.plot()` for better control, especially when working with subplots.

---

## Figure and Axes Structure

Understanding the structure of a Matplotlib figure is crucial for creating professional visualizations.

### Components

```
Figure (Canvas)
  └── Axes (Plot Area)
        ├── xaxis / yaxis  ← ticks, tick labels, axis label
        ├── Title
        ├── Legend
        └── Artists (lines, patches, text, etc.)
```

**Figure (Canvas):**
- The entire window or page that everything is drawn on
- Can contain multiple Axes (subplots)
- Controls overall size, DPI, and background

**Axes (Plot Area):**
- The actual plot area where data is visualized
- Contains the plot, labels, legend, etc.
- A Figure can contain multiple Axes

**Axis (x/y scales):**
- The number-line-like objects that provide ticks and labels
- X-axis and Y-axis define the coordinate system

```python
# Create figure and axes explicitly
fig = plt.figure(figsize=(10, 6))  # Figure (canvas)
ax = fig.add_subplot(111)          # Axes (plot area)

# Or use the convenient method (preferred)
fig, ax = plt.subplots(figsize=(10, 6))

# Multiple axes in one figure
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 5))
ax1.plot(x, y1)
ax2.plot(x, y2)
```

### Understanding the Hierarchy

```python
# Figure level
fig = plt.figure(figsize=(10, 6), dpi=100)  # DPI = dots per inch
fig.suptitle('Figure Title', fontsize=16)    # Title for entire figure

# Axes level
ax = fig.add_subplot(111)
ax.set_title('Axes Title')                   # Title for this plot
ax.set_xlabel('X Label')
ax.set_ylabel('Y Label')

# Axis level (fine-grained control)
ax.xaxis.set_major_locator(plt.MaxNLocator(10))  # Control x-axis ticks
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda x, p: f'${x:.0f}'))  # Format y-axis
```

---

## Common Plot Types

### Line Plot

```python
x = np.linspace(0, 10, 100)
y = np.sin(x)

plt.figure(figsize=(10, 6))
plt.plot(x, y, linewidth=2, color='blue', linestyle='-', marker='o', markersize=4)
plt.xlabel('X')
plt.ylabel('Y')
plt.title('Line Plot')
plt.show()
```

**Common `linestyle` options:** `'-'` (solid), `'--'` (dashed), `'-.'` (dash-dot), `':'` (dotted)

### Scatter Plot

```python
x = np.random.randn(100)
y = np.random.randn(100)
colors = np.random.rand(100)
sizes = 1000 * np.random.rand(100)

plt.figure(figsize=(10, 6))
plt.scatter(x, y, c=colors, s=sizes, alpha=0.5, cmap='viridis')
plt.colorbar()
plt.xlabel('X')
plt.ylabel('Y')
plt.title('Scatter Plot')
plt.show()
```

### Bar Plot

```python
categories = ['A', 'B', 'C', 'D', 'E']
values = [23, 45, 56, 78, 32]

# Vertical bar
plt.figure(figsize=(10, 6))
plt.bar(categories, values, color='skyblue', edgecolor='black')
plt.xlabel('Categories')
plt.ylabel('Values')
plt.title('Bar Plot')
plt.show()

# Horizontal bar
plt.figure(figsize=(10, 6))
plt.barh(categories, values, color='lightcoral')
plt.xlabel('Values')
plt.ylabel('Categories')
plt.title('Horizontal Bar Plot')
plt.show()
```

### Histogram

```python
data = np.random.normal(100, 15, 1000)

plt.figure(figsize=(10, 6))
plt.hist(data, bins=30, color='steelblue', edgecolor='black', alpha=0.7)
plt.xlabel('Value')
plt.ylabel('Frequency')
plt.title('Histogram')
plt.show()

# Overlapping histograms (compare two groups)
data1 = np.random.normal(100, 15, 1000)
data2 = np.random.normal(120, 15, 1000)

plt.figure(figsize=(10, 6))
plt.hist(data1, bins=30, alpha=0.5, label='Group 1', color='blue')
plt.hist(data2, bins=30, alpha=0.5, label='Group 2', color='red')
plt.xlabel('Value')
plt.ylabel('Frequency')
plt.title('Multiple Histograms')
plt.legend()
plt.show()
```

### Box Plot

```python
data = [np.random.normal(0, std, 100) for std in range(1, 5)]

plt.figure(figsize=(10, 6))
plt.boxplot(data, labels=['Group 1', 'Group 2', 'Group 3', 'Group 4'])
plt.ylabel('Value')
plt.title('Box Plot')
plt.show()
```

> 💡 **Box Plot anatomy:** The box shows Q1–Q3 (IQR), the line inside is the median, whiskers extend to 1.5×IQR, and points beyond are outliers.

### Pie Chart

```python
sizes = [30, 25, 25, 20]
labels = ['A', 'B', 'C', 'D']
colors = ['#ff9999', '#66b3ff', '#99ff99', '#ffcc99']

plt.figure(figsize=(8, 8))
plt.pie(sizes, labels=labels, colors=colors, autopct='%1.1f%%', startangle=90)
plt.title('Pie Chart')
plt.show()
```

---

## Customizing Plots

### Colors and Styles

```python
x = np.linspace(0, 10, 100)

plt.figure(figsize=(10, 6))
plt.plot(x, np.sin(x), color='red', linewidth=2, linestyle='--', label='sin(x)')
plt.plot(x, np.cos(x), color='#0066CC', linewidth=2, linestyle='-',
         marker='o', markersize=3, label='cos(x)')
plt.xlabel('X', fontsize=12, fontweight='bold')
plt.ylabel('Y', fontsize=12, fontweight='bold')
plt.title('Customized Plot', fontsize=14, fontweight='bold')
plt.legend(fontsize=10, loc='upper right')
plt.grid(True, alpha=0.3)
plt.show()
```

### Axis Customization

```python
fig, ax = plt.subplots(figsize=(10, 6))
ax.plot(x, y)
ax.set_xlabel('X axis', fontsize=12)
ax.set_ylabel('Y axis', fontsize=12)
ax.set_title('Customized Axes', fontsize=14)
ax.set_xlim(0, 10)
ax.set_ylim(-1.5, 1.5)
ax.set_xticks(np.arange(0, 11, 2))
ax.set_yticks(np.arange(-1, 2, 0.5))
ax.grid(True, linestyle='--', alpha=0.5)
plt.show()
```

### Text and Annotations

```python
plt.figure(figsize=(10, 6))
plt.plot(x, y)
plt.title('Plot with Annotations')

# Add plain text
plt.text(5, 0.5, 'Mid Point', fontsize=12, ha='center')

# Add annotation with arrow
plt.annotate('Maximum', xy=(np.pi/2, 1), xytext=(4, 0.5),
             arrowprops=dict(arrowstyle='->', color='red', lw=2),
             fontsize=12)
plt.show()
```

---

## Subplots and Multiple Plots

### Basic Subplots

```python
fig, axes = plt.subplots(2, 2, figsize=(12, 10))

# Plot 1: Line
axes[0, 0].plot(np.linspace(0, 10, 100), np.sin(np.linspace(0, 10, 100)))
axes[0, 0].set_title('Sine Wave')
axes[0, 0].grid(True)

# Plot 2: Scatter
axes[0, 1].scatter(np.random.randn(100), np.random.randn(100))
axes[0, 1].set_title('Scatter Plot')

# Plot 3: Bar
axes[1, 0].bar(['A', 'B', 'C', 'D'], [10, 20, 30, 40])
axes[1, 0].set_title('Bar Plot')

# Plot 4: Histogram
axes[1, 1].hist(np.random.normal(0, 1, 1000), bins=30)
axes[1, 1].set_title('Histogram')

plt.tight_layout()
plt.show()
```

### GridSpec (Advanced Layout)

```python
from matplotlib.gridspec import GridSpec

fig = plt.figure(figsize=(12, 8))
gs = GridSpec(2, 2, figure=fig)

ax1 = fig.add_subplot(gs[0, :])  # Top row — spans all columns
ax2 = fig.add_subplot(gs[1, 0])  # Bottom left
ax3 = fig.add_subplot(gs[1, 1])  # Bottom right

ax1.plot(np.linspace(0, 10, 100), np.sin(np.linspace(0, 10, 100)))
ax1.set_title('Wide Plot')

ax2.scatter(np.random.randn(50), np.random.randn(50))
ax2.set_title('Scatter')

ax3.bar(['A', 'B', 'C'], [10, 20, 30])
ax3.set_title('Bar')

plt.tight_layout()
plt.show()
```

---

## Seaborn Introduction

Seaborn is a high-level visualization library built on top of Matplotlib, designed for quick, attractive, and informative statistical plots with minimal code.

**Key Features:**
- **Statistical plots**: Built-in statistical visualizations
- **Beautiful defaults**: Attractive styling out of the box
- **Easy to use**: Less code for common plots
- **Integration**: Works seamlessly with Pandas DataFrames

### Setup

```python
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

# Set style globally
sns.set_style("whitegrid")  # Options: darkgrid, whitegrid, dark, white, ticks
sns.set_palette("husl")     # Set color palette
sns.set_context("notebook") # Options: paper, notebook, talk, poster
```

---

## Seaborn Relational Plots

Relational plots visualize relationships between two numeric variables, helping identify trends, correlations, and distributions.

### scatterplot()

Displays individual data points to show correlation:

```python
tips = sns.load_dataset('tips')

# Basic scatter
plt.figure(figsize=(10, 6))
sns.scatterplot(data=tips, x='total_bill', y='tip')
plt.title('Total Bill vs Tip')
plt.show()

# With hue (color by category)
sns.scatterplot(data=tips, x='total_bill', y='tip', hue='sex')
plt.show()

# With size (bubble chart effect)
sns.scatterplot(data=tips, x='total_bill', y='tip',
                hue='sex', size='size', sizes=(50, 200))
plt.show()

# With style (different markers per group)
sns.scatterplot(data=tips, x='total_bill', y='tip',
                hue='sex', style='time')
plt.show()
```

**Use Cases:**
- Visualize relationships between numeric variables (e.g., price vs. sales)
- Discover patterns or clusters in data
- Identify correlations between features

### lineplot()

Shows continuous trends or summary lines over time or categories:

```python
# Basic line
sns.lineplot(data=tips, x='total_bill', y='tip')
plt.show()

# With hue
sns.lineplot(data=tips, x='total_bill', y='tip', hue='sex')
plt.show()

# With markers
sns.lineplot(data=tips, x='total_bill', y='tip',
             hue='sex', marker='o', markersize=8)
plt.show()

# Time series example
dates = pd.date_range('2024-01-01', periods=100, freq='D')
ts_data = pd.DataFrame({
    'date': dates,
    'value': np.cumsum(np.random.randn(100))
})
sns.lineplot(data=ts_data, x='date', y='value')
plt.show()
```

**Use Cases:**
- Show trends over time
- Compare trends across categories
- Visualize continuous relationships with confidence intervals

### Styling & Palettes

```python
# Styles
sns.set_style("darkgrid")    # Dark background with grid
sns.set_style("whitegrid")   # White background with grid
sns.set_style("dark")        # Dark background, no grid
sns.set_style("white")       # White background, no grid
sns.set_style("ticks")       # Minimal style with ticks

# Palettes
sns.set_palette("pastel")      # Soft, muted colors
sns.set_palette("deep")        # Deep, rich colors
sns.set_palette("muted")       # Muted, balanced colors
sns.set_palette("bright")      # Bright, vibrant colors
sns.set_palette("dark")        # Dark colors
sns.set_palette("colorblind")  # Colorblind-friendly

# Custom palette
custom_palette = ["#FF6B6B", "#4ECDC4", "#45B7D1", "#FFA07A"]
sns.set_palette(custom_palette)

# Reset to matplotlib defaults
sns.reset_orig()
```

---

## Seaborn Categorical Plots

Categorical plots are used when one variable is categorical (like gender, day, or class). They help compare counts, averages, and spread across different groups.

### barplot()

Displays the **mean** (with confidence interval) of a numeric variable per category:

```python
# Basic
sns.barplot(data=tips, x='day', y='total_bill')
plt.title('Average Bill by Day')
plt.show()

# With hue
sns.barplot(data=tips, x='day', y='total_bill', hue='sex')
plt.show()

# Horizontal
sns.barplot(data=tips, x='total_bill', y='day', orient='h')
plt.show()

# Custom estimator (median instead of mean)
sns.barplot(data=tips, x='day', y='total_bill', estimator=np.median)
plt.show()
```

### countplot()

Shows the **frequency** (count) of observations per category:

```python
# Basic
sns.countplot(data=tips, x='day')
plt.title('Frequency by Day')
plt.show()

# With hue
sns.countplot(data=tips, x='day', hue='sex')
plt.show()

# Horizontal
sns.countplot(data=tips, y='day')
plt.show()

# Custom order
sns.countplot(data=tips, x='day', order=['Thur', 'Fri', 'Sat', 'Sun'])
plt.show()
```

### boxplot()

Visualizes **median, quartiles, and outliers**:

```python
# Basic
sns.boxplot(data=tips, x='day', y='total_bill')
plt.title('Bill Distribution by Day')
plt.show()

# With hue
sns.boxplot(data=tips, x='day', y='total_bill', hue='sex')
plt.show()

# Horizontal
sns.boxplot(data=tips, x='total_bill', y='day', orient='h')
plt.show()

# Custom style
sns.boxplot(data=tips, x='day', y='total_bill',
            palette='Set2', linewidth=2)
plt.show()
```

### violinplot()

Combines boxplot with **distribution shape** (KDE) for deeper insight:

```python
# Basic
sns.violinplot(data=tips, x='day', y='total_bill')
plt.title('Distribution Shape by Day')
plt.show()

# With hue
sns.violinplot(data=tips, x='day', y='total_bill', hue='sex')
plt.show()

# Split violins (great for comparing two groups side by side)
sns.violinplot(data=tips, x='day', y='total_bill',
               hue='sex', split=True)
plt.show()

# Inner plot type: 'box', 'quart', 'point', 'stick'
sns.violinplot(data=tips, x='day', y='total_bill', inner='box')
plt.show()
```

### stripplot()

Displays **all raw data points**, even if they overlap:

```python
# Basic
sns.stripplot(data=tips, x='day', y='total_bill')
plt.show()

# With jitter (reduce overlap)
sns.stripplot(data=tips, x='day', y='total_bill', jitter=True)
plt.show()

# Custom style
sns.stripplot(data=tips, x='day', y='total_bill',
              size=4, alpha=0.5, palette='Set2')
plt.show()
```

### swarmplot()

Like stripplot but **prevents point overlap** (better for smaller datasets):

```python
# Basic
sns.swarmplot(data=tips, x='day', y='total_bill')
plt.show()

# With hue
sns.swarmplot(data=tips, x='day', y='total_bill', hue='sex')
plt.show()

# Best combo: violin + swarm
sns.violinplot(data=tips, x='day', y='total_bill', inner=None)
sns.swarmplot(data=tips, x='day', y='total_bill',
              color='white', edgecolor='gray')
plt.show()
```

### Categorical Plot Comparison

| Plot | Shows | Best For |
|------|-------|----------|
| `barplot` | Mean + CI | Comparing averages |
| `countplot` | Count/frequency | Category distribution |
| `boxplot` | Median, IQR, outliers | Distribution summary |
| `violinplot` | Box + density shape | Full distribution shape |
| `stripplot` | All points (overlap ok) | Small datasets, raw view |
| `swarmplot` | All points (no overlap) | Small datasets, clean view |

**Practical Use Cases:**
- Compare customer spending by weekday
- Analyze score distributions by gender
- Visualize category frequencies in survey or sales data

---

## Seaborn Distribution Plots

Distribution plots visualize how data values are spread, concentrated, or distributed across a range.

### histplot()

Modern histogram with options for bins and KDE overlay:

```python
# Basic histogram
sns.histplot(data=tips, x='total_bill', bins=30)
plt.title('Distribution of Total Bill')
plt.show()

# With KDE overlay
sns.histplot(data=tips, x='total_bill', bins=30, kde=True)
plt.show()

# With hue (compare groups)
sns.histplot(data=tips, x='total_bill', hue='sex', bins=30, kde=True)
plt.show()

# Stacked histogram
sns.histplot(data=tips, x='total_bill', hue='sex',
             bins=30, multiple='stack')
plt.show()

# Step histogram (outline only)
sns.histplot(data=tips, x='total_bill', bins=30,
             element='step', fill=False)
plt.show()
```

### kdeplot()

Displays a **smooth density curve** representing data distribution:

```python
# Basic KDE
sns.kdeplot(data=tips, x='total_bill')
plt.title('Density Curve of Total Bill')
plt.show()

# With shading
sns.kdeplot(data=tips, x='total_bill', shade=True)
plt.show()

# With hue
sns.kdeplot(data=tips, x='total_bill', hue='sex', shade=True)
plt.show()

# 2D KDE (bivariate)
sns.kdeplot(data=tips, x='total_bill', y='tip', shade=True)
plt.show()

# Cumulative distribution (CDF)
sns.kdeplot(data=tips, x='total_bill', cumulative=True)
plt.show()
```

### rugplot()

Adds small **tick marks for individual observations** along an axis:

```python
# Basic rug
sns.rugplot(data=tips, x='total_bill')
plt.show()

# Combined with histogram + KDE
sns.histplot(data=tips, x='total_bill', bins=30, kde=True)
sns.rugplot(data=tips, x='total_bill', height=0.05, alpha=0.5)
plt.show()
```

### ⚠️ Note on distplot()

**`distplot()` is deprecated** — replaced by `histplot()` and `kdeplot()`:

```python
# ❌ OLD (deprecated):
# sns.distplot(tips['total_bill'])

# ✅ NEW:
sns.histplot(data=tips, x='total_bill', kde=True)  # histogram + KDE
# or
sns.kdeplot(data=tips, x='total_bill')             # KDE only
```

**Practical Use Cases:**
- Analyze distribution of prices, tips, or sales amounts
- Compare value distributions across categories
- Identify skewness, outliers, and data patterns

---

## Seaborn Regression & Mixed Plots

Regression and mixed plots visualize relationships between variables along with fitted trend lines.

### regplot()

Displays a scatterplot with a **fitted regression line**:

```python
# Basic regression
sns.regplot(data=tips, x='total_bill', y='tip')
plt.title('Total Bill vs Tip with Regression Line')
plt.show()

# With 95% confidence interval (default)
sns.regplot(data=tips, x='total_bill', y='tip', ci=95)
plt.show()

# Without confidence interval
sns.regplot(data=tips, x='total_bill', y='tip', ci=None)
plt.show()

# Customize markers
sns.regplot(data=tips, x='total_bill', y='tip',
            marker='+', scatter_kws={'s': 50, 'alpha': 0.5})
plt.show()

# Polynomial regression (order=2)
sns.regplot(data=tips, x='total_bill', y='tip', order=2)
plt.show()
```

### lmplot()

Like `regplot()` but supports **hue grouping and faceting**:

```python
# Basic
sns.lmplot(data=tips, x='total_bill', y='tip')
plt.show()

# With hue (group by category)
sns.lmplot(data=tips, x='total_bill', y='tip', hue='sex')
plt.show()

# Faceted by column (separate plots per day)
sns.lmplot(data=tips, x='total_bill', y='tip',
           col='day', col_wrap=2)
plt.show()

# Multiple facets (row and column)
sns.lmplot(data=tips, x='total_bill', y='tip',
           row='sex', col='day')
plt.show()

# Custom markers per group
sns.lmplot(data=tips, x='total_bill', y='tip',
           hue='sex', markers=['o', 's'], palette='Set1')
plt.show()
```

### jointplot()

Combines **scatter + marginal histograms/KDE** for joint and marginal distributions:

```python
# Basic (scatter + histograms)
sns.jointplot(data=tips, x='total_bill', y='tip')
plt.show()

# KDE
sns.jointplot(data=tips, x='total_bill', y='tip', kind='kde')
plt.show()

# With regression line
sns.jointplot(data=tips, x='total_bill', y='tip', kind='reg')
plt.show()

# Hex bins (good for large datasets)
sns.jointplot(data=tips, x='total_bill', y='tip', kind='hex')
plt.show()

# Scatter with KDE on margins
sns.jointplot(data=tips, x='total_bill', y='tip',
              kind='scatter', marginal_kws=dict(bins=30, kde=True))
plt.show()
```

### pairplot()

Shows **pairwise relationships** among all numeric variables in a dataset:

```python
iris = sns.load_dataset('iris')

# Basic pair plot
sns.pairplot(iris)
plt.show()

# With hue (color by species)
sns.pairplot(iris, hue='species')
plt.show()

# KDE on diagonal
sns.pairplot(iris, hue='species', diag_kind='kde')
plt.show()

# Select specific variables
sns.pairplot(iris, vars=['sepal_length', 'sepal_width', 'petal_length'],
             hue='species')
plt.show()

# Custom markers per group
sns.pairplot(iris, hue='species', markers=['o', 's', 'D'])
plt.show()
```

**Practical Use Cases:**
- Analyze bill-vs-tip correlations
- Visualize gender-based spending trends
- Explore all feature relationships in datasets like Iris

---

## Seaborn Matrix & Styling Plots

Matrix/grid plots are great for exploring correlations, patterns, and relationships within datasets.

### heatmap()

Displays correlations or pivot tables as **colored grids**:

```python
# Correlation heatmap
correlation = tips.select_dtypes(include=[np.number]).corr()
plt.figure(figsize=(10, 8))
sns.heatmap(correlation, annot=True, cmap='coolwarm', center=0)
plt.title('Correlation Heatmap')
plt.show()

# Pivot table heatmap
pivot = tips.pivot_table(values='total_bill', index='day', columns='time')
sns.heatmap(pivot, annot=True, fmt='.2f', cmap='YlOrRd')
plt.title('Total Bill Heatmap')
plt.show()

# Fully customized
sns.heatmap(correlation, annot=True, fmt='.2f',
            cmap='coolwarm', center=0,
            square=True, linewidths=1, cbar_kws={'label': 'Correlation'})
plt.show()
```

### clustermap()

Adds **hierarchical clustering** to heatmaps for grouping similar variables:

```python
# Basic clustermap
sns.clustermap(correlation, annot=True, cmap='coolwarm', center=0)
plt.show()

# Custom distance metric
sns.clustermap(correlation, metric='euclidean', method='ward')
plt.show()

# Full control
sns.clustermap(correlation, annot=True, fmt='.2f',
               cmap='coolwarm', figsize=(10, 8),
               row_cluster=True, col_cluster=True)
plt.show()
```

### FacetGrid

Creates **small multiples** — same plot repeated across subsets of data:

```python
# FacetGrid by day and sex
g = sns.FacetGrid(tips, col='day', row='sex', height=4)
g.map(sns.scatterplot, 'total_bill', 'tip')
g.add_legend()
plt.show()
```

### Styling Options

Control global styling for all Seaborn plots:

```python
# Style options
sns.set_style("white")        # Clean white background
sns.set_style("dark")         # Dark background
sns.set_style("whitegrid")    # White with grid (default)
sns.set_style("darkgrid")     # Dark with grid
sns.set_style("ticks")        # Minimal with axis ticks only

# Palette options
sns.set_palette("Set2")       # Qualitative (categorical data)
sns.set_palette("pastel")     # Soft colors
sns.set_palette("deep")       # Deep, rich colors
sns.set_palette("colorblind") # Accessible to colorblind users

# Context (scales plot elements)
sns.set_context("paper")      # Smallest — for print
sns.set_context("notebook")   # Default — for Jupyter
sns.set_context("talk")       # Larger — for presentations
sns.set_context("poster")     # Largest — for posters

# Combine settings
sns.set_theme(style="whitegrid", palette="Set2", context="notebook")

# Reset to matplotlib defaults
sns.reset_orig()
```

---

## Plotly and Dash for Interactive Visualizations

Plotly is a powerful library for creating interactive, publication-quality visualizations. Dash is a framework built on Plotly for building analytical web applications.

### Why Plotly?

**Advantages:**
- **Interactive**: Hover, zoom, pan, and filter built-in
- **Publication Quality**: Professional-looking plots
- **Web-Ready**: Works in Jupyter notebooks and web browsers
- **Multiple Backends**: Python, R, JavaScript
- **Rich Features**: 3D plots, animations, subplots

### Installation

```python
pip install plotly dash
```

### Plotly Express (Simple API)

Plotly Express provides a simple, high-level interface for creating interactive plots.

#### Basic Plots

```python
import plotly.express as px
import pandas as pd
import numpy as np

# Sample data
df = pd.DataFrame({
    'x': np.random.randn(100),
    'y': np.random.randn(100),
    'category': np.random.choice(['A', 'B', 'C'], 100),
    'size': np.random.rand(100) * 100
})

# Scatter plot
fig = px.scatter(df, x='x', y='y', color='category', size='size',
                 title='Interactive Scatter Plot',
                 labels={'x': 'X Axis', 'y': 'Y Axis'})
fig.show()

# Line plot (time series)
df_time = pd.DataFrame({
    'date': pd.date_range('2024-01-01', periods=100),
    'value': np.cumsum(np.random.randn(100))
})
fig = px.line(df_time, x='date', y='value', title='Time Series')
fig.show()

# Bar chart
df_bar = pd.DataFrame({'category': ['A', 'B', 'C', 'D'],
                        'value': [10, 20, 15, 25]})
fig = px.bar(df_bar, x='category', y='value', title='Bar Chart')
fig.show()

# Histogram
fig = px.histogram(df, x='x', nbins=30, title='Histogram')
fig.show()

# Box plot
fig = px.box(df, x='category', y='y', title='Box Plot')
fig.show()
```

#### Advanced Plotly Express

```python
# 3D Scatter
fig = px.scatter_3d(df, x='x', y='y', z='size', color='category')
fig.show()

# Faceted plots (small multiples)
fig = px.scatter(df, x='x', y='y', facet_col='category',
                 title='Faceted Scatter Plot')
fig.show()

# Correlation heatmap
correlation_matrix = df[['x', 'y', 'size']].corr()
fig = px.imshow(correlation_matrix, text_auto=True, aspect="auto",
                title='Correlation Heatmap')
fig.show()

# Animated chart (over time)
fig = px.scatter(df_time, x='date', y='value',
                 animation_frame='date', title='Animated Time Series')
fig.show()
```

### Plotly Graph Objects (Advanced Control)

For more granular control, use Plotly Graph Objects:

```python
import plotly.graph_objects as go
from plotly.subplots import make_subplots

# Create figure with multiple traces
fig = go.Figure()

fig.add_trace(go.Scatter(
    x=[1, 2, 3, 4],
    y=[10, 11, 12, 13],
    mode='lines+markers',
    name='Line 1'
))

fig.add_trace(go.Bar(
    x=['A', 'B', 'C'],
    y=[5, 10, 15],
    name='Bar'
))

# Update layout
fig.update_layout(
    title='Custom Combined Plot',
    xaxis_title='X Axis',
    yaxis_title='Y Axis',
    hovermode='x unified'
)

fig.show()

# Subplots with Plotly
fig = make_subplots(rows=1, cols=2, subplot_titles=('Scatter', 'Bar'))

fig.add_trace(go.Scatter(x=df['x'], y=df['y'], mode='markers'), row=1, col=1)
fig.add_trace(go.Bar(x=['A', 'B', 'C'], y=[10, 20, 30]), row=1, col=2)

fig.update_layout(title='Plotly Subplots')
fig.show()
```

### Dash for Web Applications

Dash allows you to build interactive web applications with Plotly.

#### Basic Dash App

```python
import dash
from dash import dcc, html
from dash.dependencies import Input, Output
import plotly.express as px
import pandas as pd

tips = pd.read_csv('tips.csv')  # or sns.load_dataset('tips')
app = dash.Dash(__name__)

app.layout = html.Div([
    html.H1("Tips Dashboard"),

    dcc.Dropdown(
        id='day-filter',
        options=[{'label': d, 'value': d} for d in tips['day'].unique()],
        value='Sat',
        clearable=False
    ),

    dcc.Graph(id='scatter-plot')
])

@app.callback(
    Output('scatter-plot', 'figure'),
    Input('day-filter', 'value')
)
def update_chart(selected_day):
    filtered = tips[tips['day'] == selected_day]
    fig = px.scatter(filtered, x='total_bill', y='tip',
                     color='sex', title=f'Tips on {selected_day}')
    return fig

if __name__ == '__main__':
    app.run_server(debug=True)
```

### Matplotlib vs Seaborn vs Plotly

| Feature | Matplotlib | Seaborn | Plotly |
|---------|-----------|---------|--------|
| **Control** | Full | Medium | Medium |
| **Ease of use** | Low | High | High |
| **Interactivity** | None | None | Full |
| **Statistical** | Manual | Built-in | Limited |
| **Web/Dashboard** | No | No | Yes (Dash) |
| **Best for** | Custom/publication | EDA/stats | Dashboards/apps |

---

## Saving Figures

### Saving Matplotlib/Seaborn Plots

```python
plt.figure(figsize=(10, 6))
plt.plot(np.linspace(0, 10, 100), np.sin(np.linspace(0, 10, 100)))
plt.title('Sine Wave')

plt.savefig('plot.png', dpi=300, bbox_inches='tight')  # PNG (raster)
plt.savefig('plot.pdf', bbox_inches='tight')           # PDF (vector)
plt.savefig('plot.svg', bbox_inches='tight')           # SVG (vector)
plt.savefig('plot.jpg', dpi=300, quality=95)           # JPEG

# Transparent background (for presentations)
plt.savefig('plot.png', transparent=True, dpi=300)

plt.show()
```

### Publication-Quality Figures

```python
fig, ax = plt.subplots(figsize=(8, 6), dpi=300)
ax.plot(x, y, linewidth=2)
ax.set_xlabel('X', fontsize=14)
ax.set_ylabel('Y', fontsize=14)
ax.set_title('Publication Quality Figure', fontsize=16)
plt.tight_layout()
plt.savefig('publication_figure.png', dpi=300, bbox_inches='tight')
plt.show()
```

### Saving Plotly Figures

```python
import plotly.express as px

fig = px.scatter(tips, x='total_bill', y='tip')

# Save as HTML (interactive)
fig.write_html('plot.html')

# Save as static image (requires kaleido: pip install kaleido)
fig.write_image('plot.png', width=1200, height=800, scale=2)
fig.write_image('plot.pdf')
fig.write_image('plot.svg')
```

---

## Key Takeaways

1. **Matplotlib** → Full control; use for custom or publication-grade plots
2. **Seaborn** → Beautiful defaults; use for statistical EDA plots
3. **Plotly** → Interactivity; use for dashboards and web apps
4. **Subplots** → Use `GridSpec` for complex layouts, `subplots()` for simple grids
5. **Always label** → Add axis labels, titles, and legends to every plot
6. **Save high-quality** → Use `dpi=300` and `bbox_inches='tight'` for crisp figures

---

## Common Patterns

### Pattern 1: EDA Visualization Pipeline

```python
def plot_eda(df, target_col):
    """Create comprehensive EDA visualizations for a numeric column."""
    fig, axes = plt.subplots(2, 2, figsize=(14, 10))
    fig.suptitle(f'EDA: {target_col}', fontsize=16)

    # Distribution
    sns.histplot(df[target_col], bins=30, kde=True, ax=axes[0, 0])
    axes[0, 0].set_title('Distribution')

    # Box plot
    axes[0, 1].boxplot(df[target_col].dropna())
    axes[0, 1].set_title('Box Plot')

    # Scatter vs index
    axes[1, 0].scatter(range(len(df)), df[target_col], alpha=0.4, s=10)
    axes[1, 0].set_title('Values over Index')

    # Correlation heatmap
    numeric_df = df.select_dtypes(include=[np.number])
    sns.heatmap(numeric_df.corr(), annot=True, cmap='coolwarm', ax=axes[1, 1])
    axes[1, 1].set_title('Correlation Matrix')

    plt.tight_layout()
    plt.show()

# Usage
tips = sns.load_dataset('tips')
plot_eda(tips, 'total_bill')
```

### Pattern 2: Quick Comparison Plot

```python
def compare_groups(df, x_col, y_col, group_col):
    """Compare distributions across groups using multiple plot types."""
    fig, axes = plt.subplots(1, 3, figsize=(18, 6))

    sns.boxplot(data=df, x=group_col, y=y_col, ax=axes[0])
    axes[0].set_title('Box Plot')

    sns.violinplot(data=df, x=group_col, y=y_col, ax=axes[1])
    axes[1].set_title('Violin Plot')

    sns.barplot(data=df, x=group_col, y=y_col, ax=axes[2])
    axes[2].set_title('Bar Plot (Mean ± CI)')

    plt.suptitle(f'{y_col} by {group_col}')
    plt.tight_layout()
    plt.show()

# Usage
compare_groups(tips, 'day', 'total_bill', 'day')
```

---

## Practice Exercises

### Exercise 1: Basic Line Plot

**Task:** Create a line plot of `y = x²` from -5 to 5 with proper labels and grid.

**Solution:**

```python
x = np.linspace(-5, 5, 100)
y = x**2

plt.figure(figsize=(10, 6))
plt.plot(x, y, linewidth=2, color='blue')
plt.xlabel('X', fontsize=12)
plt.ylabel('Y = X²', fontsize=12)
plt.title('Quadratic Function', fontsize=14)
plt.grid(True, alpha=0.3)
plt.show()
```

### Exercise 2: Multiple Subplots

**Task:** Create a 2×2 subplot grid with four different plot types.

**Solution:**

```python
fig, axes = plt.subplots(2, 2, figsize=(12, 10))

x = np.linspace(0, 10, 100)

axes[0, 0].plot(x, np.sin(x), color='blue')
axes[0, 0].set_title('Sine Wave')

axes[0, 1].scatter(np.random.randn(100), np.random.randn(100), alpha=0.5)
axes[0, 1].set_title('Scatter Plot')

axes[1, 0].bar(['A', 'B', 'C'], [10, 20, 30], color='skyblue', edgecolor='black')
axes[1, 0].set_title('Bar Plot')

axes[1, 1].hist(np.random.normal(0, 1, 1000), bins=30, color='steelblue', edgecolor='black')
axes[1, 1].set_title('Histogram')

plt.tight_layout()
plt.show()
```

### Exercise 3: EDA Dashboard with Seaborn

**Task:** Load the `tips` dataset and create a 4-panel EDA dashboard.

**Solution:**

```python
tips = sns.load_dataset('tips')

fig, axes = plt.subplots(2, 2, figsize=(14, 10))

sns.histplot(data=tips, x='total_bill', kde=True, ax=axes[0, 0])
axes[0, 0].set_title('Distribution of Total Bill')

sns.boxplot(data=tips, x='day', y='total_bill', ax=axes[0, 1])
axes[0, 1].set_title('Bill by Day')

sns.scatterplot(data=tips, x='total_bill', y='tip', hue='sex', ax=axes[1, 0])
axes[1, 0].set_title('Tip vs Bill')

sns.barplot(data=tips, x='day', y='total_bill', hue='sex', ax=axes[1, 1])
axes[1, 1].set_title('Average Bill by Day & Gender')

plt.tight_layout()
plt.show()
```

### Exercise 4: Correlation Heatmap

**Task:** Generate a random DataFrame and visualize its correlation matrix.

**Solution:**

```python
df = pd.DataFrame(np.random.randn(100, 4), columns=['A', 'B', 'C', 'D'])
# Add some correlation artificially
df['B'] = df['A'] * 0.8 + np.random.randn(100) * 0.2
df['C'] = df['A'] * -0.5 + np.random.randn(100) * 0.3

correlation = df.corr()

plt.figure(figsize=(8, 6))
sns.heatmap(correlation, annot=True, fmt='.2f', cmap='coolwarm',
            center=0, square=True, linewidths=1)
plt.title('Correlation Heatmap')
plt.show()
```

### Exercise 5: Interactive Plotly Chart

**Task:** Create an interactive scatter plot with Plotly Express using the `tips` dataset.

**Solution:**

```python
import plotly.express as px

tips = sns.load_dataset('tips')

fig = px.scatter(tips, x='total_bill', y='tip',
                 color='sex', size='size',
                 facet_col='time',
                 hover_data=['day', 'smoker'],
                 title='Tips: Interactive Exploration')
fig.show()
```

---

## Next Steps

- Practice creating different plot types with your own datasets
- Experiment with customization (palettes, styles, annotations)
- Try Plotly for interactive dashboards
- Build a Dash app for a real project
- Move to [02-introduction-to-ml](../02-introduction-to-ml/README.md) for ML concepts

**Remember:** Good visualizations tell a story — always think about what message you want to convey!

