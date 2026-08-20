# NumPy — Master Revision Notes

## 1. What is NumPy?

**NumPy (Numerical Python)** is a fundamental Python library for **scientific computing and data analysis**.

### Main uses

- Working with **arrays and matrices**
    
- Fast **mathematical operations**
    
- Vectorized computations
    
- Statistical operations
    
- Foundation for **Pandas, Machine Learning and Deep Learning**
    

> 🧠 **Mental Model:**  
> **NumPy = Fast numerical operations + arrays/matrices**

---

# 2. Installing NumPy

### Using `requirements.txt`

```bash
pip install -r requirements.txt
```

Add:

```text
numpy
```

### Direct installation

```bash
pip install numpy
```

Inside Jupyter Notebook:

```python
!pip install numpy
```

---

# 3. Importing NumPy

```python
import numpy as np
```

`np` is the conventional **alias** for NumPy.

```python
np.array(...)
np.arange(...)
np.zeros(...)
```

> 🧠 **Remember:** `np` is only an alias; it does not represent a different library.

---

# 4. Creating NumPy Arrays

## 1D Array

```python
arr1 = np.array([1, 2, 3, 4, 5])

print(arr1)
print(type(arr1))
```

Output type:

```text
numpy.ndarray
```

### Important distinction

A NumPy 1D array:

```python
[1 2 3 4 5]
```

is **not** considered a row or column matrix.

Its shape is:

```python
arr1.shape
# (5,)
```

> 🧠 `(5,)` → one-dimensional array containing **5 elements**.

---

# 5. 2D Arrays

A nested list creates a 2D array:

```python
arr2 = np.array([
    [1, 2, 3, 4, 5],
    [2, 3, 4, 5, 6]
])

print(arr2)
print(arr2.shape)
```

Shape:

```text
(2, 5)
```

Meaning:

- `2` → rows
    
- `5` → columns
    

### Dimension recognition

```text
1D → [1, 2, 3]

2D → [[1, 2, 3],
      [4, 5, 6]]

3D → [[[...]]]
```

> 🧠 **Number of nested levels ≈ number of dimensions.**

---

# 6. Reshaping Arrays

Use:

```python
arr.reshape(rows, columns)
```

Example:

```python
arr = np.array([1, 2, 3, 4, 5])

arr.reshape(1, 5)
```

Result:

```text
[[1 2 3 4 5]]
```

Shape:

```text
(1, 5)
```

→ **1 row × 5 columns**

### Another possibility

```python
arr.reshape(5, 1)
```

→ **5 rows × 1 column**

### Critical rule

The **total number of elements must remain unchanged**.

```python
arr.reshape(1, 5)   # ✅ 5 elements
arr.reshape(5, 1)   # ✅ 5 elements
arr.reshape(1, 4)   # ❌ 4 elements
```

> 🧠 **Rows × Columns = Total elements**

For example:

```text
1 × 5 = 5
5 × 1 = 5
```

Don't interpret `(1, 5)` as `1 + 5`.

---

# 7. `np.arange()`

Creates evenly spaced values within a specified range.

### Syntax

```python
np.arange(start, stop, step)
```

Example:

```python
arr = np.arange(0, 10, 2)
print(arr)
```

Output:

```text
[0 2 4 6 8]
```

### Important

The `stop` value is generally **excluded**.

```python
np.arange(0, 10, 2)
# 0, 2, 4, 6, 8
```

Can then reshape:

```python
arr.reshape(5, 1)
```

---

# 8. Useful NumPy Array Creation Functions

## `np.ones()`

Creates an array filled with `1`.

```python
np.ones((3, 4))
```

→ 3 rows × 4 columns, all values `1`.

### Use case

Useful for quickly initializing matrices, including situations such as model/weight initialization.

---

## `np.zeros()`

Creates an array filled with `0`.

```python
np.zeros((3, 4))
```

### Use case

Useful when an array/matrix needs to be initialized with zeros.

---

## `np.eye()`

Creates an **identity matrix**.

```python
np.eye(3)
```

Output:

```text
[[1 0 0]
 [0 1 0]
 [0 0 1]]
```

For a non-square shape:

```python
np.eye(3, 2)
```

The diagonal contains `1`s and remaining elements are `0`.

> 🧠 **Identity matrix = diagonal → 1, everything else → 0**

---

# 9. NumPy Array Attributes

Given:

```python
arr = np.array([
    [1, 2, 3],
    [4, 5, 6]
])
```

| Attribute   | Purpose                    | Example        |
| ----------- | -------------------------- | -------------- |
| `.shape`    | Dimensions                 | `arr.shape`    |
| `.ndim`     | Number of dimensions       | `arr.ndim`     |
| `.size`     | Total number of elements   | `arr.size`     |
| `.dtype`    | Data type                  | `arr.dtype`    |
| `.itemsize` | Bytes used by each element | `arr.itemsize` |

Example:

```python
print(arr.shape)
print(arr.ndim)
print(arr.size)
print(arr.dtype)
print(arr.itemsize)
```

### Example interpretation

For:

```python
arr = np.array([
    [1, 2, 3],
    [4, 5, 6]
])
```

```text
shape  → (2, 3)
ndim   → 2
size   → 6
dtype  → integer type
```

> ⚠️ `dtype` can vary by platform/system, e.g. `int32` or `int64`.

---

# 10. Vectorized Operations

One of NumPy's biggest advantages is **vectorization**.

Instead of manually looping through elements, operations can be applied directly to entire arrays.

```python
r1 = np.array([1, 2, 3, 4, 5])
r2 = np.array([10, 20, 30, 40, 50])
```

## Element-wise Addition

```python
r1 + r2
```

Result:

```text
[11 22 33 44 55]
```

Each element is operated on according to its **same index**.

```text
1 + 10
2 + 20
3 + 30
...
```

---

## Element-wise Subtraction

```python
r1 - r2
```

## Element-wise Multiplication

```python
r1 * r2
```

## Element-wise Division

```python
r1 / r2
```

### Core idea

```text
Array1 op Array2
       ↓
same-index elements operate together
```

> 🧠 **Vectorization = operations on the whole array without explicitly writing loops.**

The result is also a NumPy array.

---

# 11. Universal Functions — `ufunc`

NumPy provides **universal functions (ufuncs)** that operate element-wise across arrays.

Given:

```python
arr = np.array([1, 2, 3, 4, 5, 6])
```

## Square Root

```python
np.sqrt(arr)
```

## Exponential

```python
np.exp(arr)
```

## Sine

```python
np.sin(arr)
```

## Natural Log

```python
np.log(arr)
```

### Key idea

```text
np.function(array)
        ↓
operation applied element-wise
```

Other NumPy mathematical functions can be used similarly.

---

# 12. Array Indexing

NumPy uses **zero-based indexing**.

Given:

```python
arr = np.array([
    [1, 2, 3, 4],
    [5, 6, 7, 8],
    [9, 10, 11, 12]
])
```

Shape:

```text
(3, 4)
```

Access an individual element:

```python
arr[0, 0]
```

Output:

```text
1
```

### Index structure

```python
arr[row_index, column_index]
```

For example:

```python
arr[1, 2]
```

→ `7`

because:

```text
row 1 → [5, 6, 7, 8]
column 2 → 7
```

> 🧠 Always think **`[row, column]`** for 2D arrays.

---

# 13. Array Slicing

### General syntax

```python
array[row_start:row_end, column_start:column_end]
```

The ending index is **excluded**.

Given:

```python
arr = np.array([
    [1, 2, 3, 4],
    [5, 6, 7, 8],
    [9, 10, 11, 12]
])
```

## Select rows from index 1 onward

```python
arr[1:, :]
```

Result:

```text
[[ 5  6  7  8]
 [ 9 10 11 12]]
```

## Select columns 2 onward

```python
arr[:, 2:]
```

Result:

```text
[[ 3  4]
 [ 7  8]
 [11 12]]
```

## Select first two columns

```python
arr[:, 0:2]
```

Result:

```text
[[1 2]
 [5 6]
 [9 10]]
```

## Select columns 2 and 3 from rows 1 onward

```python
arr[1:, 2:]
```

Result:

```text
[[ 7  8]
 [11 12]]
```

---

# 14. Slicing Patterns to Remember

| Syntax         | Meaning                         |
| -------------- | ------------------------------- |
| `arr[1:]`      | From index 1 to end             |
| `arr[:2]`      | From beginning up to index 2    |
| `arr[:, 2:]`   | All rows, columns from 2 onward |
| `arr[:, :2]`   | All rows, first 2 columns       |
| `arr[1:, 1:3]` | Rows from 1, columns 1–2        |

> 🧠 **Slice = `start : stop` → stop is excluded.**

---

# 15. Modifying Array Elements

NumPy arrays are mutable.

Given:

```python
arr = np.array([
    [1, 2, 3],
    [4, 5, 6]
])
```

Change one element:

```python
arr[0, 0] = 100
```

Now:

```text
[[100   2   3]
 [  4   5   6]]
```

### Modify a slice

```python
arr[1:, :] = 100
```

All elements from row `1` onward become `100`.

> 🧠 **Indexing selects → assignment modifies.**

---

# 16. Statistical Operations

NumPy provides many statistical functions.

Given:

```python
data = np.array([1, 2, 3, 4, 5])
```

### Mean

```python
np.mean(data)
```

### Median

```python
np.median(data)
```

### Standard Deviation

```python
np.std(data)
```

### Variance

```python
np.var(data)
```

|Function|Purpose|
|---|---|
|`np.mean()`|Mean|
|`np.median()`|Median|
|`np.std()`|Standard deviation|
|`np.var()`|Variance|

---

# 17. Normalization

A common statistical/ML operation is **standardization/normalization** to transform data toward:

```text
Mean = 0
Standard deviation = 1
```

### Formula

[  
z = \frac{x-\mu}{\sigma}  
]

Where:

- `x` → original value
    
- `μ` → mean
    
- `σ` → standard deviation
    

### NumPy implementation

```python
data = np.array([1, 2, 3, 4, 5])

mean = np.mean(data)
std = np.std(data)

normalized_data = (data - mean) / std
```

> 🧠 **Standardization = subtract mean → divide by standard deviation**

This is also associated with the **standard normal distribution** and is widely used in ML preprocessing.

---

# 18. Logical Operations / Boolean Indexing

NumPy allows conditions to be directly applied to arrays.

```python
data = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
```

### Condition

```python
data > 5
```

Produces a Boolean array:

```text
[False False False False False True True True True True]
```

### Retrieve matching values

```python
data[data > 5]
```

Output:

```text
[6 7 8 9 10]
```

> 🧠 **Boolean indexing = condition → True/False mask → select matching elements**

---

# 19. Multiple Conditions

For NumPy arrays, use:

- `&` → logical AND
    
- `|` → logical OR
    

### Example: values from 5 to 8

```python
data[(data >= 5) & (data <= 8)]
```

Output:

```text
[5 6 7 8]
```

### Important syntax rule

Put each condition inside parentheses:

```python
(data >= 5) & (data <= 8)
```

❌ Don't use Python's:

```python
and
```

for element-wise NumPy array conditions.

Use:

```python
&
```

Similarly:

```python
(data < 3) | (data > 8)
```

for OR.

---

# 20. NumPy vs Python Lists

| Python List                               | NumPy Array                                  |
| ----------------------------------------- | -------------------------------------------- |
| General-purpose collection                | Numerical computing structure                |
| Slower for large numerical operations     | Highly optimized for numerical operations    |
| Manual loops often needed                 | Vectorized operations                        |
| Less suitable for matrix operations       | Excellent for arrays/matrices                |
| Rich numerical functionality not built in | Extensive mathematical/statistical functions |

> 🧠 **Data Science mental model:**  
> **Python List → general data collection**  
> **NumPy Array → numerical computation**

---

# 21. NumPy in Data Science / ML

NumPy is foundational because many data-science operations involve:

```text
Data
 ↓
NumPy Arrays
 ↓
Mathematical / Statistical Operations
 ↓
Machine Learning
 ↓
Deep Learning
```

Common applications:

- Matrix operations
    
- Feature manipulation
    
- Statistical calculations
    
- Normalization
    
- Numerical preprocessing
    
- Model computations
    
- Weight initialization
    
- Tensor/matrix-related operations
    

---

# ⚡ Quick Revision Sheet

```python
import numpy as np
```

### Create

```python
np.array([1, 2, 3])
np.array([[1, 2], [3, 4]])
np.arange(0, 10, 2)
np.ones((3, 4))
np.zeros((3, 4))
np.eye(3)
```

### Inspect

```python
arr.shape
arr.ndim
arr.size
arr.dtype
arr.itemsize
```

### Reshape

```python
arr.reshape(2, 3)
```

**Rule:** total elements must remain the same.

### Element-wise operations

```python
a + b
a - b
a * b
a / b
```

### Universal functions

```python
np.sqrt(a)
np.exp(a)
np.sin(a)
np.log(a)
```

### Statistics

```python
np.mean(a)
np.median(a)
np.std(a)
np.var(a)
```

### Indexing

```python
arr[row, column]
```

### Slicing

```python
arr[row_start:row_end, col_start:col_end]
```

### Boolean filtering

```python
arr[arr > 5]
arr[(arr >= 5) & (arr <= 8)]
```

### Normalization

```python
mean = np.mean(data)
std = np.std(data)
normalized = (data - mean) / std
```

---

# 🧠 NumPy Interview/Exam Traps

|Question|Remember|
|---|---|
|What is NumPy?|Scientific/numerical computing library|
|Standard alias?|`np`|
|NumPy array type?|`numpy.ndarray`|
|`(5,)` means?|1D array with 5 elements|
|`(2,5)` means?|2 rows × 5 columns|
|Reshape requirement?|Total elements must remain unchanged|
|`np.arange(0,10,2)`?|`[0,2,4,6,8]`|
|Identity matrix?|`np.eye()`|
|Total elements?|`.size`|
|Dimensions?|`.ndim`|
|Data type?|`.dtype`|
|Element size?|`.itemsize`|
|Element-wise operation?|`+`, `-`, `*`, `/`|
|Square root?|`np.sqrt()`|
|Mean?|`np.mean()`|
|Standard deviation?|`np.std()`|
|Variance?|`np.var()`|
|2D indexing?|`[row, column]`|
|AND condition?|`&`|
|OR condition?|`|
|Boolean filtering?|`arr[condition]`|
|Standardization formula?|`(x - mean) / std`|

## 🔑 Final Mental Model

**NumPy = Arrays + Speed + Vectorization + Mathematics**

```text
Create
  ↓
np.array / arange / ones / zeros / eye
  ↓
Inspect
  ↓
shape / ndim / size / dtype
  ↓
Transform
  ↓
reshape
  ↓
Compute
  ↓
Vectorized + Universal Functions
  ↓
Select
  ↓
Indexing + Slicing + Boolean Filtering
  ↓
Analyze
  ↓
Mean / Median / Std / Variance / Normalization
```

**High-priority for exams/interviews:** `array()`, `shape`, `reshape()`, `arange()`, `ones()`, `zeros()`, `eye()`, array attributes, vectorization, ufuncs, indexing/slicing, Boolean indexing, statistical functions, and normalization.


# Pandas — Master Revision Notes

## 1. What is Pandas?

**Pandas** is a powerful Python library used for:

- Data manipulation
    
- Data analysis
    
- Data cleaning
    

It provides two primary data structures:

|Structure|Dimension|Description|
|---|--:|---|
|**Series**|1D|Array-like object, similar to a single column|
|**DataFrame**|2D|Tabular structure with rows and columns|

A DataFrame is **mutable** and can be **heterogeneous**, meaning different columns can contain different types of data.

---

## 2. Installing Pandas

Using `requirements.txt`:

```bash
pip install -r requirements.txt
```

Add:

```text
pandas
```

Or install directly:

```bash
pip install pandas
```

---

## 3. Importing Pandas

```python
import pandas as pd
```

`pd` is the conventional alias for Pandas.

---

# Pandas Series

## 4. What is a Series?

A **Pandas Series** is a **one-dimensional array-like object** that can hold any data type.

It can be thought of as a **single column in a table**.

```text
Series
   ↓
1-dimensional
   ↓
Single column
```

---

## 5. Creating a Series from a List

```python
import pandas as pd

data = [1, 2, 3, 4, 5]

series = pd.Series(data)

print(series)
```

Output:

```text
0    1
1    2
2    3
3    4
4    5
dtype: int64
```

The numbers on the left are the **default indexes**.

```python
type(series)
```

Output:

```text
pandas.core.series.Series
```

---

## 6. Creating a Series from a Dictionary

A Series can also be created from a dictionary:

```python
data = {
    "a": 1,
    "b": 2,
    "c": 3
}

series_dict = pd.Series(data)

print(series_dict)
```

Output:

```text
a    1
b    2
c    3
```

When a dictionary is converted to a Series:

- **Keys → indexes**
    
- **Values → Series values**
    

---

## 7. Creating a Series with Custom Indexes

You can provide your own indexes:

```python
data = [10, 20, 30]

indexes = ["a", "b", "c"]

series = pd.Series(
    data,
    index=indexes
)

print(series)
```

Output:

```text
a    10
b    20
c    30
```

Syntax:

```python
pd.Series(data, index=indexes)
```

The number of indexes should correspond to the number of data elements.

---

# Pandas DataFrame

## 8. What is a DataFrame?

A **DataFrame** is a:

- Two-dimensional
    
- Mutable
    
- Potentially heterogeneous
    
- Tabular data structure
    
- With labeled rows and columns
    

It is similar to a **table in a database or an Excel sheet**.

```text
DataFrame
    ↓
Rows + Columns
    ↓
Complete table
```

---

## 9. Creating a DataFrame from a Dictionary of Lists

Example:

```python
data = {
    "name": ["Krish", "John", "Jack"],
    "age": [25, 30, 45],
    "city": ["Bangalore", "New York", "Florida"]
}

df = pd.DataFrame(data)

print(df)
```

Output:

```text
    name  age       city
0  Krish   25  Bangalore
1   John   30   New York
2   Jack   45    Florida
```

Here:

- Dictionary keys → **column names**
    
- Lists → **column values**
    
- Pandas automatically creates row indexes
    

All columns should contain the appropriate number of values for the records being created.

---

## 10. Creating a DataFrame from a List of Dictionaries

Each dictionary can represent one row:

```python
data = [
    {
        "name": "Krish",
        "age": 32,
        "city": "Bangalore"
    },
    {
        "name": "John",
        "age": 34,
        "city": "Bangalore"
    },
    {
        "name": "Jack",
        "age": 32,
        "city": "Bangalore"
    }
]

df = pd.DataFrame(data)

print(df)
```

Here:

**Each dictionary → one row**

This differs from a **dictionary of lists**, where each key represents a column.

---

## 11. Converting a DataFrame to NumPy

A DataFrame can be converted to a NumPy array:

```python
np.array(df)
```

The resulting NumPy array contains the underlying data, without the DataFrame's row indexes and column names.

---

# Reading Data

## 12. Reading a CSV File

In real-world data analysis, DataFrames are often created from datasets such as CSV files.

```python
df = pd.read_csv("sales_data.csv")
```

Important function:

```python
pd.read_csv()
```

This reads the CSV file and creates a DataFrame.

The transcript also mentions functions for other formats, such as:

```python
pd.read_excel()
```

for Excel files.

---

## 13. `df.head()`

To view the first few rows:

```python
df.head()
```

By default, it displays the first **5 rows**.

You can specify the number:

```python
df.head(5)
```

---

## 14. `df.tail()`

To view the last few rows:

```python
df.tail()
```

By default, it displays the last **5 rows**.

You can specify the number:

```python
df.tail(5)
```

---

# Accessing Data

## 15. Selecting a Column

Suppose:

```python
df = pd.DataFrame({
    "name": ["Krish", "John", "Jack"],
    "age": [25, 30, 45],
    "city": ["Bangalore", "New York", "Florida"]
})
```

To select the `name` column:

```python
df["name"]
```

Selecting a single column returns a **Series**.

```python
type(df["name"])
```

→ `pandas.core.series.Series`

---

## 16. `.loc[]` — Label-Based Access

`loc` is used to access data using **labels**.

### Select a row

```python
df.loc[0]
```

This selects the row with label `0`.

### Select a specific value

```python
df.loc[0, "name"]
```

This selects the `name` value from row `0`.

General form:

```python
df.loc[row_label, column_label]
```

---

## 17. `.iloc[]` — Integer Position-Based Access

`iloc` accesses data using **integer positions**.

### Select a row

```python
df.iloc[0]
```

→ first row.

### Select a specific element

```python
df.iloc[0, 0]
```

This means:

- Row position = `0`
    
- Column position = `0`
    

Another example:

```python
df.iloc[0, 1]
```

→ first row, second column.

General form:

```python
df.iloc[row_position, column_position]
```

---

## 18. `.at[]` — Specific Element Using Labels

`at` is used to access a **specific cell using row and column labels**.

Syntax:

```python
df.at[row_label, column_label]
```

Example:

```python
df.at[1, "age"]
```

If row `1` has age `30`, the result is:

```text
30
```

Another example:

```python
df.at[2, "name"]
```

→ `Jack`

---

## 19. `.iat[]` — Specific Element Using Positions

`iat` accesses a specific cell using **integer positions**.

Syntax:

```python
df.iat[row_position, column_position]
```

Example:

```python
df.iat[2, 2]
```

This accesses the third row and third column.

---

# Data Manipulation

## 20. Adding a New Column

A new column can be added directly:

```python
df["salary"] = [50000, 60000, 70000]
```

General syntax:

```python
df["new_column"] = values
```

The new column is added to the DataFrame.

---

## 21. Removing a Column

Use the `drop()` method:

```python
df.drop("salary", axis=1)
```

Here:

```text
salary → column to remove
axis=1 → operate on columns
```

Pandas uses:

```text
axis=0 → rows
axis=1 → columns
```

If `axis=1` is omitted, `drop()` operates on the row index by default. Therefore, trying to drop a column name without specifying the column axis can result in an error.

---

## 22. `inplace=True`

By default:

```python
df.drop("salary", axis=1)
```

does not permanently modify the original DataFrame.

To apply the change directly:

```python
df.drop(
    "salary",
    axis=1,
    inplace=True
)
```

With `inplace=True`, the existing DataFrame is modified.

---

## 23. Modifying an Existing Column

Pandas allows operations to be applied directly to an entire column.

Example:

```python
df["age"] = df["age"] + 1
```

If the original values are:

```text
25
30
45
```

they become:

```text
26
31
46
```

The operation is applied to all values in the column.

---

## 24. Dropping a Row

Rows can also be removed with `drop()`.

```python
df.drop(0)
```

This removes the row with index `0` from the returned result.

To permanently remove it:

```python
df.drop(0, inplace=True)
```

Since the default axis is `0`, this operates on rows.

---

# Statistical Analysis

## 25. `df.describe()`

`describe()` provides a **statistical summary of numerical columns**.

```python
df.describe()
```

Typical output contains:

|Statistic|Meaning|
|---|---|
|`count`|Number of records|
|`mean`|Average|
|`std`|Standard deviation|
|`min`|Minimum|
|`25%`|25th percentile|
|`50%`|50th percentile / median|
|`75%`|75th percentile|
|`max`|Maximum|

For a sales dataset, this can provide statistics for numerical fields such as:

- Unit sold
    
- Unit price
    
- Total revenue
    

The transcript ends this lesson by using `describe()` as the starting point for further data manipulation, which is covered in the following lesson.