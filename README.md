# Feature Engineering Using Snowflake and Feature Store
---

## 🌟 Objective

The purpose of this project is to extract, transform, and store customer-level features from raw retail transaction data using Snowflake. The transformed data is organized into a Feature Store that can later be used in machine learning workflows.

---

## 📂 Project Structure

```
Vistora_Project/
├── eda_retail_transactions.ipynb     # EDA notebook
├── feature_store_sql_steps.sql       # SQL feature engineering scripts (step-by-step)
├── retail_data.csv                   # Raw retail dataset (from UCI ML Repository)
├── project_explanation.pdf           # Project explanation PDF
└── README.md                         # This file
```

---

## 🔗 Useful Links

* 📽️ [Project Walkthrough Video](https://your-video-link.com)
* 📄 [Dataset Source (UCI Online Retail)](https://archive.ics.uci.edu/ml/datasets/online+retail)
* 📘 [Project Explanation PDF](./project_explanation.pdf)

---

## 1. 🧠 Introduction to Feature Engineering

Feature engineering transforms raw data into meaningful inputs that improve ML model performance. We used SQL-based transformations to generate customer-level insights such as order frequency, spending behavior, recency, and more.

---

## 2. ❄️ Using Snowflake for Data Storage & Processing

The dataset was uploaded to a Snowflake table named `retail_transactions`. All SQL-based transformations were run directly within Snowflake. Data was accessed through both the Snowsight UI and Python via Snowflake connectors.

---

## 3. 🏠 Feature Store Concepts

A Feature Store stores engineered features consistently for both training and inference. In this project, the `retail_feature_store` Snowflake table acts as our feature store, storing one row per customer.

---

## 4. 🔍 Feature Engineering: Step-by-Step

Each feature was created using SQL and added to the feature store table incrementally.

### 🔹 1. `CustomerID`, `Country`

* Extracted directly from raw data.
* Used to uniquely identify customers and provide geographical grouping.

### 🔹 2. One-Hot Encoded Countries

```sql
IFF(Country = 'United Kingdom', 1, 0) AS is_UK
```

* Encoded top countries as binary columns (`is_UK`, `is_Germany`, etc.)
* Helps machine learning models handle categorical data effectively.

### 🔹 3. `total_orders`

```sql
COUNT(DISTINCT InvoiceNo)
```

* Number of purchases made.
* Indicates customer frequency.

### 🔹 4. `total_items`

```sql
SUM(Quantity)
```

* Total quantity purchased.
* Measures volume of purchases.

### 🔹 5. `total_spent`

```sql
SUM(Quantity * UnitPrice)
```

* Total revenue from a customer.
* Critical for customer value segmentation.

### 🔹 6. `avg_items_per_order`

```sql
SUM(Quantity) / COUNT(DISTINCT InvoiceNo)
```

* Average items bought per order.
* Indicates shopping behavior.

### 🔹 7. `avg_revenue_per_order`

```sql
SUM(Quantity * UnitPrice) / COUNT(DISTINCT InvoiceNo)
```

* Average monetary value per order.
* Useful for tiering customers.

### 🔹 8. `clv_per_day`

```sql
SUM(Quantity * UnitPrice) / DATEDIFF('day', MIN(InvoiceDate), DATEADD(day, 1, MAX(InvoiceDate)))
```

* Proxy for customer lifetime value over time.
* Indicates consistency of spending.

### 🔹 9. `first_purchase_date`, `last_purchase_date`

```sql
MIN(InvoiceDate), MAX(InvoiceDate)
```

* Used to derive time-based features like recency and lifetime.

### 🔹 10. `recency_days`

```sql
DATEDIFF('day', MAX(InvoiceDate), CURRENT_DATE)
```

* Days since last purchase.
* Key indicator of churn.

### 🔹 11. `customer_lifetime_days`

```sql
DATEDIFF('day', MIN(InvoiceDate), MAX(InvoiceDate))
```

* Length of time customer has been active.

### 🔹 12. `return_ratio`

```sql
SUM(IFF(Quantity < 0, 1, 0)) / COUNT(*)
```

* Fraction of returned transactions.
* Identifies risky or unsatisfied customers.

---

## 5. 📅 Final Feature Store Table

Each row in `retail_feature_store` represents a customer and their behavior. This structure is ideal for feeding into ML models for:

* Churn prediction
* Customer segmentation
* CLV forecasting

Example query for ML access:

```sql
SELECT * FROM retail_feature_store WHERE CustomerID = '12345';
```

---

## 6. 📓 About the EDA Notebook

The file `eda_retail_transactions.ipynb` includes:

* Connection to Snowflake from Python
* Preview of the raw data
* Handling missing values
* Visualizations: quantity distribution, sales trends, and top countries
* SQL used to extract subsets of data for validation

This notebook provided critical insight before feature engineering began.

---

## 7. 🌟 Deliverables

| Item                    | Status      |
| ----------------------- | ----------- |
| EDA Notebook (`.ipynb`) | ✅ Completed |
| SQL Scripts             | ✅ Completed |
| Feature Store Table     | ✅ Created   |
| Project Explanation PDF | ✅ Included  |
| Walkthrough Video       | ✅ Linked    |
| README                  | ✅ This file |

---

## 🙋‍♂️ Author

**Anshul Buxy**
Engineering Student | ML Enthusiast

```bash
Email: anshul.buxy@example.com
GitHub: github.com/anshulbuxy
```
