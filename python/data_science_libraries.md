# Python Data Science Libraries (Basics)

Python has become the de facto language for data science due to its simplicity, vast ecosystem of libraries, and strong community support. This note introduces three fundamental libraries for data manipulation, analysis, and visualization: NumPy, Pandas, and Matplotlib.

## 1. NumPy (Numerical Python)

NumPy is the foundational package for numerical computing in Python. It provides support for large, multi-dimensional arrays and matrices, along with a collection of high-level mathematical functions to operate on these arrays.

### Key Features:
-   **`ndarray`:** A fast and efficient multi-dimensional array object.
-   **Vectorized Operations:** Enables operations on entire arrays without explicit loops, leading to significantly faster execution.
-   **Mathematical Functions:** A comprehensive set of mathematical functions for array operations.

### Installation

```bash
pip install numpy
```

### Basic Usage

```python
import numpy as np

# Creating NumPy arrays
arr1 = np.array([1, 2, 3, 4, 5])
print(arr1)         # Output: [1 2 3 4 5]
print(type(arr1))   # Output: <class 'numpy.ndarray'>

arr2 = np.array([[1, 2, 3], [4, 5, 6]])
print(arr2)
# Output:
# [[1 2 3]
#  [4 5 6]]

# Array attributes
print(arr2.shape) # Output: (2, 3) (rows, columns)
print(arr2.ndim)  # Output: 2 (number of dimensions)
print(arr2.dtype) # Output: int64 (data type of elements)

# Array creation functions
zeros = np.zeros((2, 3))
ones = np.ones((3, 2))
full = np.full((2, 2), 7)
identity = np.eye(3) # Identity matrix

print("Zeros:\n", zeros)
print("Ones:\n", ones)
print("Full:\n", full)
print("Identity:\n", identity)

# Array operations (vectorized)
arr_a = np.array([1, 2, 3])
arr_b = np.array([4, 5, 6])

print(arr_a + arr_b) # Element-wise addition: [5 7 9]
print(arr_a * 2)     # Scalar multiplication: [2 4 6]
print(arr_a * arr_b) # Element-wise multiplication: [4 10 18]

# Dot product
matrix_a = np.array([[1, 2], [3, 4]])
matrix_b = np.array([[5, 6], [7, 8]])
print(np.dot(matrix_a, matrix_b))

# Indexing and Slicing
my_array = np.array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
print(my_array[0])      # First element: 0
print(my_array[2:5])    # Slice: [2 3 4]
print(my_array[:5])     # First 5 elements: [0 1 2 3 4]
print(my_array[7:])     # From index 7 to end: [7 8 9]
print(my_array[::2])    # Every other element: [0 2 4 6 8]

# Conditional indexing
print(my_array[my_array > 5]) # Output: [6 7 8 9]

# Reshaping
reshaped_arr = np.arange(9).reshape(3, 3)
print("Reshaped array:\n", reshaped_arr)
```

## 2. Pandas (Data Analysis Library)

Pandas is a powerful and flexible open-source data analysis and manipulation library. It builds on NumPy and provides easy-to-use data structures and data analysis tools, most notably `DataFrame` and `Series`.

### Key Features:
-   **`DataFrame`:** A 2-dimensional labeled data structure with columns of potentially different types (like a spreadsheet or SQL table).
-   **`Series`:** A 1-dimensional labeled array capable of holding any data type (like a single column of a DataFrame).
-   **Data Cleaning and Preparation:** Tools for handling missing data, reshaping, merging, and more.
-   **Data Exploration and Analysis:** Powerful indexing, grouping, and aggregation capabilities.
-   **I/O Tools:** Easy reading and writing of data from various formats (CSV, Excel, SQL databases, JSON).

### Installation

```bash
pip install pandas
```

### Basic Usage

```python
import pandas as pd
import numpy as np

# Creating a Series
s = pd.Series([1, 3, 5, np.nan, 6, 8])
print("Series:\n", s)

# Creating a DataFrame
data = {'Name': ['Alice', 'Bob', 'Charlie', 'David'],
        'Age': [25, 30, 35, 28],
        'City': ['New York', 'London', 'Paris', 'New York']}
df = pd.DataFrame(data)
print("DataFrame:\n", df)

# Reading data from CSV
# df_csv = pd.read_csv('my_data.csv')

# Basic DataFrame operations
print("\nDataFrame Info:")
print(df.info()) # Summary of DataFrame
print("\nDataFrame Description:")
print(df.describe()) # Statistical summary of numerical columns

# Selecting data
print("\nSelect 'Name' column:")
print(df['Name'])

print("\nSelect multiple columns:")
print(df[['Name', 'Age']])

print("\nSelect rows by index (iloc):")
print(df.iloc[0]) # First row
print(df.iloc[1:3]) # Rows 1 and 2

print("\nSelect rows by label (loc):")
print(df.loc[df['City'] == 'New York'])

# Adding a new column
df['Salary'] = [50000, 60000, 75000, 55000]
print("\nDataFrame with Salary:\n", df)

# Filtering data
filtered_df = df[df['Age'] > 30]
print("\nFiltered DataFrame (Age > 30):\n", filtered_df)

# Grouping and Aggregation
print("\nAverage Age by City:")
print(df.groupby('City')['Age'].mean())

# Handling missing data (example with NaN)
df_nan = pd.DataFrame({'A': [1, 2, np.nan], 'B': [4, np.nan, 6]})
print("\nDataFrame with NaN:\n", df_nan)
print("Drop NaN:\n", df_nan.dropna())
print("Fill NaN with 0:\n", df_nan.fillna(0))
```

## 3. Matplotlib (Plotting Library)

Matplotlib is a comprehensive library for creating static, animated, and interactive visualizations in Python. It provides a wide variety of plotting options and is highly customizable.

### Key Features:
-   **Versatile Plots:** Line plots, scatter plots, bar charts, histograms, pie charts, 3D plots, etc.
-   **Customization:** Extensive control over every aspect of a plot (colors, labels, fonts, line styles).
-   **Integration:** Works well with NumPy and Pandas.

### Installation

```bash
pip install matplotlib
```

### Basic Usage

```python
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

# Line Plot
x = np.linspace(0, 10, 100) # 100 points between 0 and 10
y = np.sin(x)

plt.figure(figsize=(8, 4))
plt.plot(x, y, label='sin(x)', color='blue', linestyle='--')
plt.title('Sine Wave')
plt.xlabel('X-axis')
plt.ylabel('Y-axis')
plt.grid(True)
plt.legend()
plt.show()

# Scatter Plot
np.random.seed(42)
num_points = 50
data_x = np.random.rand(num_points) * 10
data_y = np.random.rand(num_points) * 10
colors = np.random.rand(num_points)
size = np.random.rand(num_points) * 100

plt.figure(figsize=(8, 6))
plt.scatter(data_x, data_y, c=colors, s=size, alpha=0.7, cmap='viridis')
plt.title('Scatter Plot Example')
plt.xlabel('Feature X')
plt.ylabel('Feature Y')
plt.colorbar(label='Color Intensity')
plt.show()

# Bar Chart
categories = ['A', 'B', 'C', 'D']
values = [23, 45, 56, 12]

plt.figure(figsize=(7, 5))
plt.bar(categories, values, color='skyblue')
plt.title('Bar Chart Example')
plt.xlabel('Category')
plt.ylabel('Value')
plt.show()

# Histogram
dist_data = np.random.randn(1000) # Standard normal distribution

plt.figure(figsize=(8, 5))
plt.hist(dist_data, bins=30, color='lightcoral', edgecolor='black', alpha=0.7)
plt.title('Histogram of Random Data')
plt.xlabel('Value')
plt.ylabel('Frequency')
plt.show()

# Plotting with Pandas DataFrame
df_plot = pd.DataFrame({
    'x': np.random.rand(50),
    'y': np.random.rand(50),
    'category': np.random.choice(['Group1', 'Group2'], 50)
})

plt.figure(figsize=(8, 6))
plt.scatter(df_plot['x'], df_plot['y'], c=df_plot['category'].astype('category').cat.codes, cmap='Set1')
plt.title('Scatter Plot from Pandas DataFrame')
plt.xlabel('X Value')
plt.ylabel('Y Value')
plt.show()
```

## Other Important Data Science Libraries

-   **SciPy:** (Scientific Python) Builds on NumPy and provides modules for optimization, linear algebra, integration, interpolation, special functions, FFT, signal and image processing, and other tasks common in science and engineering.
-   **Scikit-learn:** A machine learning library that provides simple and efficient tools for data mining and data analysis. It features various classification, regression, and clustering algorithms.
-   **Seaborn:** A statistical data visualization library based on Matplotlib. It provides a high-level interface for drawing attractive and informative statistical graphics.
-   **TensorFlow / PyTorch:** Deep learning frameworks for building and training neural networks.
