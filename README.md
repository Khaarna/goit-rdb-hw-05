# Relational Databases — Homework 5

## Dataset

**Name:** Olist Brazilian E-Commerce Public Dataset

**Source:** [Kaggle — Olist Brazilian E-Commerce](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

The dataset contains information about approximately 100,000 orders placed on a Brazilian e-commerce platform between 2016 and 2018.

Five CSV files were used:

| Table | Records |
|---|---:|
| Customers | 99,441 |
| Orders | 99,441 |
| Order Items | 112,650 |
| Products | 32,951 |
| Sellers | 3,095 |

The complete dataset was used without sampling.

## Tools

- Google Colab
- PostgreSQL 16 (`pgserver`)
- Python
- SQLAlchemy
- pandas
- KaggleHub

## Homework Tasks

### 1. Data Loading and Schema

Downloaded the Olist dataset using KaggleHub, created raw and typed PostgreSQL tables, and added primary keys, foreign keys, and constraints.

### 2. CTE Feature Engineering Pipeline

Created a SQL pipeline with seven CTE stages:

1. **Order Totals** — Calculate the total value of each order.
2. **RFM Features** — Calculate recency, frequency, and monetary value.
3. **Cohort** — Determine the quarter of the first order.
4. **Rolling AOV** — Calculate the 30-day rolling average order value.
5. **Cancellation Rate** — Calculate the share of cancelled orders.
6. **Category Revenue** — Calculate spending by product category.
7. **Preferred Category** — Identify the category with the highest spending.

The pipeline produces one feature record per `customer_id`.

### 3. Recursive CTE

Created a synthetic referral network with 10 customers and 9 referral relationships.

Used a recursive CTE to calculate referral depth and paths, with cycle protection using `path_ids` and a maximum depth of 50.

### 4. Materialized View

Created `mv_olist_customer_features` to store the calculated customer features.

Added a unique index on `customer_id` and successfully executed `REFRESH MATERIALIZED VIEW CONCURRENTLY`.

### 5. Performance Analysis

Used `EXPLAIN (ANALYZE, BUFFERS)` to compare the CTE pipeline and Materialized View.

| Query | Execution Time |
|---|---:|
| Full CTE pipeline (top 10) | 4,271.135 ms |
| CTE customer lookup | 717.488 ms |
| Materialized View customer lookup | 0.102 ms |

The Materialized View lookup was approximately 7,034 times faster than the CTE lookup in this execution.

These results are based on a single run and do not include the cost of creating or refreshing the Materialized View.

## Results

- Created five raw and five typed tables.
- Calculated customer features for 99,441 customer records.
- Implemented seven CTE stages with window functions.
- Created a recursive referral chain with cycle protection.
- Created and refreshed a Materialized View.
- Compared SQL execution plans and measured query performance.

## How to Run

1. Open `HW05.ipynb` in Google Colab.
2. Select **Runtime → Restart session and run all**.
3. Wait for all cells to finish executing.

The notebook automatically installs dependencies, downloads the dataset from Kaggle using KaggleHub, starts PostgreSQL, creates tables, and executes all SQL queries.

No manual database setup is required.
