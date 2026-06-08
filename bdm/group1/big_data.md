# Assignment 2: Mastering Big Data Handling

## Group Information

| Name | Matric No. | Role |
|---|---|---|
| NEO LI XIN | A23CS0253 | Student A: Baseline & Setup Lead |
| ELIJAH SHE YU SHENG | A23CS0073 | Student B: Scalability & Performance Lead |

---

## 1. Dataset Description

The dataset used in this assignment is the **Shopee Logistics Performance March** dataset from the **Kaggle Open Shopee Code League Logistics** competition. This dataset is suitable for big data handling because the extracted CSV file is larger than 700 MB, which satisfies the minimum dataset size requirement for this assignment.

| Item | Description |
|---|---|
| Dataset Name | Shopee Logistics Performance March |
| Source | Kaggle - Open Shopee Code League Logistics |
| File Name | `delivery_orders_march.csv` |
| Compressed File | `delivery_orders_march.csv.zip` |
| Compressed Size | Approximately 381 MB |
| Extracted CSV Size | Approximately 721 MB |
| Number of Records | 3,176,313 rows |
| Number of Columns | 6 columns |
| Domain | E-commerce Logistics |

The dataset contains delivery order records for Shopee logistics operations. It includes information such as order ID, pickup timestamp, first delivery attempt timestamp, second delivery attempt timestamp, buyer address, and seller address. Since the dataset contains millions of rows and text-heavy address columns, it is useful for testing memory usage, loading performance, and scalable data processing strategies.

The columns in the dataset are:

| Column | Description |
|---|---|
| `orderid` | Unique order identifier |
| `pick` | Timestamp when the order was picked up |
| `1st_deliver_attempt` | Timestamp of the first delivery attempt |
| `2nd_deliver_attempt` | Timestamp of the second delivery attempt, if available |
| `buyeraddress` | Buyer's address |
| `selleraddress` | Seller's address |

---

## 2. Library Choices

This assignment uses three Python libraries. Pandas is used as the compulsory baseline library, while Dask and Polars are selected as scalable libraries.

| Library | Role | Reason for Selection |
|---|---|---|
| Pandas | Baseline library | Pandas is widely used for data analysis and is required as the baseline for comparison. |
| Dask | Scalable library | Dask can process large CSV files in partitions and execute operations in parallel. |
| Polars | Scalable library | Polars is designed for high-performance data processing and supports lazy query optimisation. |

Pandas is simple and familiar, but it normally loads data into memory directly. This can become a limitation when the dataset is large. Dask improves scalability by breaking the dataset into partitions. Polars improves performance through its Rust-based execution engine and lazy evaluation.

---

## 3. Data Loading and Initial Inspection

Before applying optimisation strategies, the dataset was inspected using Pandas. A small sample was loaded first to understand the data structure without consuming too much memory.

The initial inspection checked:

- Number of rows and columns
- Column names
- Data types
- Missing values
- First few records

The dataset has **3,176,313 rows** and **6 columns**. The address columns are text-based and consume more memory compared to numeric columns. The second delivery attempt column contains many missing values because not all orders require a second delivery attempt.

Example Pandas inspection code:

```python
sample_df = pd.read_csv(csv_path, nrows=10000)
print(sample_df.shape)
print(sample_df.dtypes)
print(sample_df.isnull().sum())
sample_df.head()
```

---

## 4. Big Data Handling Strategies

Five big data handling strategies were implemented in this assignment. The first four strategies were implemented using Pandas, while the fifth strategy used scalable libraries.

---

### 4.1 Strategy 1: Load Less Data

The first strategy is to load only the required columns instead of loading the full dataset. The dataset contains long text columns such as `buyeraddress` and `selleraddress`. These columns are useful for address-level analysis, but they are not needed for delivery time performance analysis.

By using the `usecols` parameter, only the required numeric columns were loaded:

```python
use_cols = ["orderid", "pick", "1st_deliver_attempt", "2nd_deliver_attempt"]
df_less = pd.read_csv(csv_path, usecols=use_cols)
```

This strategy reduces memory usage because unnecessary columns are not loaded into RAM. It also improves loading time because Pandas reads fewer columns from the CSV file.

---

### 4.2 Strategy 2: Chunking

Chunking means reading the dataset in smaller parts instead of loading the whole CSV file at once. This is useful when the dataset is too large to fit comfortably in memory.

Example implementation:

```python
chunk_size = 100000
total_rows = 0
missing_second_attempt = 0

for chunk in pd.read_csv(csv_path, usecols=use_cols, chunksize=chunk_size):
    total_rows += len(chunk)
    missing_second_attempt += chunk["2nd_deliver_attempt"].isnull().sum()
```

In this assignment, chunking was used to count total rows and missing second delivery attempts. This proves that the file can be processed without loading the entire dataset into memory at the same time.

---

### 4.3 Strategy 3: Data Type Optimisation

Pandas may automatically use larger data types than necessary, such as `int64` or `float64`. These default types can increase memory usage. Data type optimisation reduces memory usage by assigning smaller suitable types.

Example implementation:

```python
dtype_map = {
    "orderid": "int64",
    "pick": "int32",
    "1st_deliver_attempt": "float32",
    "2nd_deliver_attempt": "float32"
}

df_optimised = pd.read_csv(csv_path, usecols=use_cols, dtype=dtype_map)
```

The timestamp columns were stored using smaller numeric types where suitable. This reduces the memory footprint and makes later processing more efficient.

---

### 4.4 Strategy 4: Sampling

Sampling selects a smaller representative subset of the dataset. This is useful during early development because it allows the code to be tested quickly before running on the full dataset.

Example implementation:

```python
df_sample = pd.read_csv(csv_path, usecols=use_cols).sample(frac=0.05, random_state=42)
```

A 5% sample was used for rapid testing. Sampling does not replace full-data processing, but it helps speed up debugging, prototyping, and exploratory analysis.

---

### 4.5 Strategy 5: Parallel Processing with Scalable Libraries

The fifth strategy uses scalable libraries to process the dataset more efficiently. Dask and Polars were used in this assignment.

Dask reads the dataset in partitions and processes operations in parallel. It is useful when the dataset is too large to fit into memory or when computation should be distributed across multiple CPU cores.

Polars uses a Rust-based engine and lazy execution. With lazy execution, Polars builds an optimised query plan before running the operation. This can reduce unnecessary work and improve processing speed.

The same analysis task was performed using Pandas, Dask, and Polars:

1. Load selected columns.
2. Count total rows.
3. Count missing values in `2nd_deliver_attempt`.
4. Calculate average delivery time between pickup and first delivery attempt.

---

## 5. Comparative Analysis

The comparative analysis measured execution time and RAM usage for Pandas, Dask, and Polars. The same processing task was used for all three libraries to make the comparison fair.

### 5.1 Comparison Table

The following result is based on a local runtime test. Exact values may be slightly different when the notebook is executed in Google Colab because runtime specifications can vary.

| Library | Execution Time (seconds) | RAM Change (MB) | Rows Processed | Missing Second Attempts | Average Delivery Time (hours) |
|---|---:|---:|---:|---:|---:|
| Pandas | 9.381 | 142.734 | 3,176,313 | 1,819,311 | 104.449 |
| Dask | 7.175 | 213.133 | 3,176,313 | 1,819,311 | 104.449 |
| Polars | 3.796 | 939.348 | 3,176,313 | 1,819,311 | 104.449 |

### 5.2 Discussion of Results

Pandas was easy to implement and provided a clear baseline. However, Pandas generally loads data directly into memory, so it can become less suitable when the dataset grows much larger.

Dask completed the task faster than Pandas in this test. This is because Dask processes the CSV file using partitions and can execute multiple tasks in parallel. Dask is especially useful when working with datasets that are larger than available memory.

Polars achieved the fastest execution time in this test. This is mainly because Polars is built using Rust and supports lazy execution. Lazy execution allows Polars to optimise the query plan before running it, such as reading only the required columns and avoiding unnecessary intermediate operations.

Although Polars had the highest RAM change in this particular test, it still had the best processing speed. This shows that performance should not be judged using only one metric. Execution time, memory usage, ease of implementation, and scalability must all be considered together.

---

## 6. Conclusion and Reflection

This assignment demonstrated that handling a large dataset requires careful strategy selection. A normal `read_csv()` approach may work for a 721 MB dataset, but it can be slow and memory-intensive. By using strategies such as loading fewer columns, chunking, data type optimisation, and sampling, the dataset can be processed more efficiently.

Pandas is useful as a baseline because it is simple and widely used. However, Dask and Polars are more suitable for scalable processing. Dask is useful when the dataset needs to be divided into partitions, while Polars is useful when fast query execution is required.

If the dataset increased to **1 TB**, Pandas would no longer be suitable because the data would likely exceed available memory. Dask would be more practical because it can process data in partitions, but a 1 TB production workload may still require a distributed system such as Apache Spark, cloud-based data processing, or a data warehouse. Polars may still be useful for fast processing on a powerful machine, but distributed architecture would be needed for reliable large-scale production processing.

From this assignment, we learned that big data handling is not only about choosing a faster library. It also requires understanding memory limits, data types, file structure, parallel processing, and workflow design. The best solution depends on the size of the dataset, the type of analysis, and the available computing resources.

---

## References

1. SECP3133 High Performance Data Processing Assignment 2 Student Guide.
2. Kaggle Open Shopee Code League Logistics Dataset.
3. Pandas Documentation.
4. Dask Documentation.
5. Polars Documentation.
