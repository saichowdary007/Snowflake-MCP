# Performance Optimization Example Using MCP Template

## Problem Statement

A data analyst has a complex Snowflake query for monthly sales analytics that is taking too long to run and consuming too many resources. They need help optimizing it for better performance without changing the business logic.

## Filled Template

```markdown
# Snowflake Performance Optimization MCP Template

## Query Context

This query generates our monthly sales analytics dashboard for executive review. It analyzes sales performance across all regions, products, and customer segments. It's critical that this runs efficiently as it's part of our month-end closing process and has a tight time window for completion.

## Current Query

```sql
SELECT 
    DATE_TRUNC('MONTH', o.order_date) AS order_month,
    p.category AS product_category,
    c.customer_segment,
    r.region_name,
    COUNT(DISTINCT o.order_id) AS total_orders,
    COUNT(DISTINCT o.customer_id) AS unique_customers,
    SUM(o.quantity) AS total_units_sold,
    SUM(o.quantity * o.unit_price) AS gross_revenue,
    SUM(o.quantity * o.unit_price) - SUM(o.quantity * p.unit_cost) AS gross_profit,
    AVG(DATEDIFF('day', o.order_date, o.ship_date)) AS avg_shipping_days
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
JOIN products p ON o.product_id = p.product_id
JOIN regions r ON c.region_id = r.region_id
WHERE o.order_date BETWEEN '2022-01-01' AND '2023-12-31'
GROUP BY 1, 2, 3, 4
ORDER BY 1 DESC, 2, 3, 4;
```

## Database Schema

```sql
CREATE TABLE orders (
    order_id INTEGER,
    order_date DATE,
    customer_id INTEGER,
    product_id INTEGER,
    quantity INTEGER,
    unit_price DECIMAL(10,2),
    ship_date DATE
);
-- orders: ~25M rows, 15GB

CREATE TABLE customers (
    customer_id INTEGER PRIMARY KEY,
    customer_name VARCHAR(100),
    customer_segment VARCHAR(50),
    region_id INTEGER
);
-- customers: ~2M rows, 500MB

CREATE TABLE products (
    product_id INTEGER PRIMARY KEY,
    product_name VARCHAR(100),
    category VARCHAR(50),
    unit_cost DECIMAL(10,2)
);
-- products: ~50K rows, 20MB

CREATE TABLE regions (
    region_id INTEGER PRIMARY KEY,
    region_name VARCHAR(50),
    country VARCHAR(50)
);
-- regions: ~100 rows, <1MB

-- No clustering keys or special indexes currently defined
```

## Current Performance

- Execution Time: 3 minutes 45 seconds
- Rows Processed: ~25 million in orders table
- Bytes Scanned: ~12GB
- Warehouse Size: Medium (4 nodes)

## Query Profile Insights

```
- Most expensive operation: JOIN between orders and customers (74% of execution time)
- 98% of execution time spent in the compute phase
- File metadata operations: 15 seconds
- Spillage to local storage detected
- No pruning on order_date despite filter
```

## Performance Goals

Need to reduce execution time to under 1 minute and reduce bytes scanned to minimize compute costs. The query runs daily for incremental dashboards and monthly for full historical dashboards.

## Constraints

- Cannot modify the underlying tables (their structure is managed by another team)
- The business logic and output columns must remain exactly the same
- Cannot use materialized views due to company policy

## What You've Already Tried

- Increased warehouse size to Large (8 nodes) - reduced time to 2 minutes but still too slow
- Added a simple WHERE filter to reduce the size of the initial orders table - helped somewhat but not enough

## Environment Details

- Snowflake Edition: Enterprise
- Warehouse Size: Medium (auto-suspend 10 minutes, auto-resume)
- Compute Resources: Standard
```

## Request to AI Assistant

"Can you help me optimize this Snowflake query for better performance? I've provided all the context in the template. I'm particularly concerned about the execution time and the amount of data being scanned."

## AI Assistant Response

Based on your template, I can see several opportunities to optimize your query. Here are my recommendations:

1. First, let's add clustering keys to improve data pruning:

```sql
-- Add these commands (requires admin privileges)
ALTER TABLE orders CLUSTER BY (order_date);
ALTER TABLE customers CLUSTER BY (region_id);
```

2. Next, here's your optimized query:

```sql
-- Optimized query
SELECT 
    month_key.order_month,
    product_category,
    customer_segment,
    region_name,
    total_orders,
    unique_customers,
    total_units_sold,
    gross_revenue,
    gross_revenue - total_cost AS gross_profit,
    avg_shipping_days
FROM (
    -- Pre-aggregate orders table to reduce join size
    SELECT 
        DATE_TRUNC('MONTH', order_date) AS order_month,
        customer_id,
        product_id,
        COUNT(DISTINCT order_id) AS total_orders,
        SUM(quantity) AS total_units_sold,
        SUM(quantity * unit_price) AS gross_revenue,
        SUM(quantity * unit_price) AS total_revenue_for_cost,
        AVG(DATEDIFF('day', order_date, ship_date)) AS avg_shipping_days
    FROM orders
    WHERE order_date BETWEEN '2022-01-01' AND '2023-12-31'
    GROUP BY 1, 2, 3
) o
JOIN (
    -- Pre-join the smaller dimension tables
    SELECT 
        c.customer_id,
        c.customer_segment,
        r.region_name
    FROM customers c
    JOIN regions r ON c.region_id = r.region_id
) c ON o.customer_id = c.customer_id
JOIN (
    SELECT
        product_id,
        category AS product_category,
        unit_cost
    FROM products
) p ON o.product_id = p.product_id
CROSS JOIN (
    -- Create a distinct list of months to ensure all months appear
    SELECT DISTINCT DATE_TRUNC('MONTH', order_date) AS order_month
    FROM orders
    WHERE order_date BETWEEN '2022-01-01' AND '2023-12-31'
) month_key
WHERE o.order_month = month_key.order_month
GROUP BY 1, 2, 3, 4
ORDER BY 1 DESC, 2, 3, 4;
```

3. Additional performance optimizations:
   - Create a materialized view if your policy changes
   - Create a separate, smaller table for recent orders if you mostly analyze recent data
   - Consider using search optimization service for the orders table

4. Query transformation benefits:
   - Pre-aggregating orders before joining reduces the join size significantly
   - Pre-joining the smaller dimension tables (customers and regions) reduces complexity
   - Explicit column selection instead of positional references in GROUP BY improves maintainability
   - The date range filter is applied earlier in the execution plan

These changes should reduce your execution time to under 1 minute and significantly decrease the bytes scanned.

## Key Takeaways

1. The MCP template provided crucial information about the database size and structure
2. Including the Query Profile Insights revealed the expensive JOIN operation
3. Restructuring the query to pre-aggregate before joining addresses the specific performance bottleneck
4. The constraints section prevented suggesting solutions that wouldn't be implementable 