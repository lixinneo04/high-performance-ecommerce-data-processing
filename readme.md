# SECP3133 Assignment 2: Mastering Big Data Handling

## Group Information

| Name | Matric No. | Role |
|---|---|---|
| ELIJAH SHE YU SHENG | A23CS0073 | Student A: Baseline & Setup Lead |
| NEO LI XIN | A23CS0253 | Student B: Scalability & Performance Lead |

## Dataset

| Item | Description |
|---|---|
| Dataset Name | Shopee Logistics Performance March |
| Source | Kaggle - Open Shopee Code League Logistics |
| File Used | `delivery_orders_march.csv` |
| Compressed File | `delivery_orders_march.csv.zip` |
| Compressed Size | Approximately 381 MB |
| Extracted CSV Size | Approximately 721 MB |
| Number of Records | 3,176,313 rows |
| Number of Columns | 6 columns |
| Domain | E-commerce Logistics |

## Libraries Used

| Library | Purpose |
|---|---|
| Pandas | Baseline data loading, inspection, and optimisation strategies |
| Dask | Scalable dataframe processing using partition-based computation |
| Polars | High-performance dataframe processing using Rust engine and lazy execution |

## Project Files

| File | Description |
|---|---|
| `big_data.md` | Main Markdown technical report |
| `big_data.ipynb` | Google Colab notebook containing all code, explanations, tables, and charts |
| `readme.md` | Project overview, group details, dataset information, and file links |

## Work Distribution

| Student | Responsibility |
|---|---|
| ELIJAH SHE YU SHENG | Dataset preparation, dataset description, Pandas baseline, Pandas strategies, GitHub setup, first half of report |
| NEO LI XIN | Dask and Polars implementation, performance testing, comparison charts, analysis, conclusion and scalability reflection |

## How to Run

1. Upload `delivery_orders_march.csv.zip` to Google Drive.
2. Open `big_data.ipynb` in Google Colab.
3. Update the `base_dir` path if your Google Drive folder is different.
4. Run all cells from top to bottom.
5. Confirm that all outputs, comparison tables, and charts are visible before submission.

## GitHub Folder Structure

```text
ass2/your_group_name/
├── big_data.md
├── big_data.ipynb
└── readme.md
```
