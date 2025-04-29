# Error Debugging Example Using MCP Template

## Problem Statement

A data analyst is trying to create a view that combines sales data with product information, but keeps receiving an error about columns not existing.

## Filled Template

```markdown
# Snowflake SQL Debugging MCP Template

## Query Context

I'm trying to create a view that combines our sales data with product information for our analytics team. The view should show sales by product category by month with some additional calculated metrics.

## Error Message

```
SQL compilation error: 
Column 'PRODUCT_CATALOG.PRODUCT_CATEGORY' does not exist
```

## Query With Error

```sql
CREATE OR REPLACE VIEW sales_by_category AS
SELECT 
    DATE_TRUNC('MONTH', s.sale_date) AS sale_month,
    p.product_category,
    COUNT(DISTINCT s.customer_id) AS unique_customers,
    COUNT(s.sale_id) AS total_sales,
    SUM(s.amount) AS total_revenue,
    SUM(s.amount) / COUNT(DISTINCT s.customer_id) AS revenue_per_customer
FROM sales s
JOIN product_catalog p ON s.product_id = p.product_id
GROUP BY 1, 2
ORDER BY 1 DESC, 2;
```

## Database Schema

```sql
CREATE TABLE sales (
    sale_id INTEGER PRIMARY KEY,
    sale_date DATE,
    customer_id INTEGER,
    product_id INTEGER,
    amount DECIMAL(10,2)
);

CREATE TABLE product_catalog (
    product_id INTEGER PRIMARY KEY,
    product_name VARCHAR(100),
    category VARCHAR(50),
    list_price DECIMAL(10,2),
    cost DECIMAL(10,2)
);
```

## Sample Data

```
-- First 3 rows of sales
-- sale_id | sale_date  | customer_id | product_id | amount
-- 1001    | 2023-07-15 | 5001        | 101        | 49.99
-- 1002    | 2023-07-16 | 5002        | 103        | 129.99
-- 1003    | 2023-07-16 | 5001        | 102        | 79.99

-- First 3 rows of product_catalog
-- product_id | product_name      | category    | list_price | cost
-- 101        | Basic Widget      | Hardware    | 49.99      | 20.00
-- 102        | Premium Widget    | Hardware    | 79.99      | 35.00
-- 103        | Software License  | Software    | 129.99     | 5.00
```

## Previous Attempts

I double-checked that both tables exist and that I can query them individually. I also tried looking at the column names in the product_catalog table using DESCRIBE, but I must be missing something.

## Environment Details

- Snowflake Edition: Enterprise
- Client Tool: Snowflake Web UI
- Query Context: Creating a View
```

## Request to AI Assistant

"Can you help me fix the error in my Snowflake SQL query? I keep getting an error about a column not existing."

## AI Assistant Response

I see the issue in your query. In your product_catalog table, the column is named "category" but in your query, you're trying to reference it as "product_category". Here's the corrected query:

```sql
CREATE OR REPLACE VIEW sales_by_category AS
SELECT 
    DATE_TRUNC('MONTH', s.sale_date) AS sale_month,
    p.category AS product_category,  -- Changed from p.product_category to p.category
    COUNT(DISTINCT s.customer_id) AS unique_customers,
    COUNT(s.sale_id) AS total_sales,
    SUM(s.amount) AS total_revenue,
    SUM(s.amount) / COUNT(DISTINCT s.customer_id) AS revenue_per_customer
FROM sales s
JOIN product_catalog p ON s.product_id = p.product_id
GROUP BY 1, 2
ORDER BY 1 DESC, 2;
```

I aliased p.category AS product_category to match your intended column name in the result set. This should resolve the "Column does not exist" error.

## Key Takeaways

1. The error message and database schema were crucial for diagnosing the issue
2. Including sample data helped confirm the actual column names
3. The debugging template provided the right context level for this type of problem
4. Previous troubleshooting attempts helped eliminate other potential causes 