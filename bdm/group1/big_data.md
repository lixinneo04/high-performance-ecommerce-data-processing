# Technical Report: Scalable Data Handling Strategies



## 1. Dataset Description

* **Dataset Name:** Shopee Logistics Performance (March)

* **Source:** [Kaggle - Shopee Code League](https://www.kaggle.com/c/open-shopee-code-league-logistic)

* **Size:** 700MB+

* **Domain:** E-commerce Logistics

* **Description:** This dataset contains high-volume delivery order logs. It is used to analyze delivery performance and latency issues across various regions.



## 2. Big Data Handling Strategies (Pandas)

In this section, we implement four optimization strategies to handle the data using the Pandas library:

1. **Load Less Data**: Using `usecols` to minimize RAM usage.

2. **Chunking**: Processing the file in segments to avoid memory overflow.

3. **Data Type Optimisation**: Reducing memory footprint through efficient type casting.

4. **Sampling**: Utilizing representative subsets for rapid prototyping.

