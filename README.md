# Week 9: SQL Practice Tasks

**Author:** Prakash Gautam

This repository contains my solutions to the seven Week 9 SQL practice tasks. The work is done in a Jupyter notebook (`Week_9.ipynb`) using Python, `pandas` and `sqlite3`. Each CSV file is loaded into a SQLite database, and each task is solved with a SQL query.

## Contents

| File | Description |
|------|-------------|
| `Week_9.ipynb` | Jupyter notebook with all seven tasks, queries and outputs |
| `sales.csv` | Sales data |
| `hr_employees.csv` | Human resources data |
| `realestate_listings.csv` | Real estate listings |
| `marketing_campaigns.csv` | Marketing campaign performance |
| `restaurant_orders.csv` | Restaurant orders |
| `clinic_visits.csv` | Healthcare clinic visits |
| `bank_customers.csv` | Bank customers |
| `bank_transactions.csv` | Bank transactions |

## Tables

Each CSV is loaded into a table in `my_data.db`:

| Table | Source file | Main columns |
|-------|-------------|--------------|
| `sales` | `sales.csv` | sale_id, sale_date, product, category, region, quantity, price |
| `employees` | `hr_employees.csv` | employee_id, name, department, city, salary, years_experience, full_time |
| `listings` | `realestate_listings.csv` | listing_id, neighborhood, property_type, bedrooms, sqft, list_price, days_on_market |
| `campaigns` | `marketing_campaigns.csv` | record_id, campaign, channel, week, impressions, clicks, spend, conversions |
| `orders` | `restaurant_orders.csv` | order_id, order_date, menu_item, server, table_size, order_total, tip |
| `visits` | `clinic_visits.csv` | visit_id, visit_date, department, doctor, patient_age, wait_minutes, visit_cost |
| `customers` | `bank_customers.csv` | customer_id, customer_name, account_type, branch, join_year |
| `transactions` | `bank_transactions.csv` | transaction_id, customer_id, transaction_date, transaction_type, amount |

## Tasks and Queries

### 1. SELECT: Sales Data
Display `sale_date`, `product` and `price` from the sales table.

```sql
SELECT sale_date, product, price
FROM sales;
```

### 2. WHERE: Human Resources
Find employees in Chicago with more than 10 years of experience using `WHERE` and `AND`.

```sql
SELECT name, department, city, salary, years_experience
FROM employees
WHERE city = 'Chicago'
  AND years_experience > 10;
```

### 3. ORDER BY: Real Estate
Display the three largest properties by square footage using `ORDER BY` and `LIMIT`.

```sql
SELECT neighborhood, property_type, list_price, sqft
FROM listings
ORDER BY sqft DESC
LIMIT 3;
```

### 4. GROUP BY: Marketing
Calculate total weekly impressions using `SUM()` and `GROUP BY`.

```sql
SELECT week, SUM(impressions) AS total_impressions
FROM campaigns
GROUP BY week
ORDER BY week;
```

### 5. HAVING: Restaurant
Find dates when total restaurant sales exceeded $400.

```sql
SELECT order_date, ROUND(SUM(order_total), 2) AS total_sales
FROM orders
GROUP BY order_date
HAVING SUM(order_total) > 400;
```

### 6. Aggregate Functions: Healthcare
Find the average patient age and longest waiting time for each doctor.

```sql
SELECT doctor,
       ROUND(AVG(patient_age), 2) AS avg_patient_age,
       MAX(wait_minutes) AS max_wait_minutes
FROM visits
GROUP BY doctor;
```

### 7. JOIN: Banking
Join the customer and transaction tables to calculate total transaction amounts by branch.

```sql
SELECT c.branch,
       ROUND(SUM(t.amount), 2) AS total_transaction_amount
FROM transactions AS t
JOIN customers AS c ON t.customer_id = c.customer_id
GROUP BY c.branch;
```

## How to Run

1. Download or clone this repository.
2. Make sure `Week_9.ipynb` and all eight CSV files are in the same folder.
3. Install the requirements if needed:
   ```
   pip install pandas jupyterlab
   ```
4. Start JupyterLab and open the notebook:
   ```
   jupyter lab
   ```
5. Choose **Kernel > Restart Kernel and Run All Cells**. The first cell creates `my_data.db` and loads every CSV into it, so the later cells can query the tables.

## Tools Used

- Python 3
- pandas
- sqlite3 (included with Python)
- JupyterLab
