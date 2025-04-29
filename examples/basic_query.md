# Basic Query Example Using Minimal MCP Template

## Problem Statement

A marketing analyst needs to find the top 5 customers by total spend in Q1 2023, along with their contact information, for a targeted email campaign.

## Filled Template

```markdown
# Minimal MCP Template for Snowflake

## Query Goal

I need to identify our top 5 customers by total spend during Q1 2023 (Jan-Mar) to send them a special loyalty promotion. I need their customer ID, name, email, and total amount spent.

## Database Schema

```sql
CREATE TABLE customers (
  customer_id INTEGER PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100),
  signup_date DATE,
  customer_segment VARCHAR(50)
);

CREATE TABLE orders (
  order_id INTEGER PRIMARY KEY,
  customer_id INTEGER REFERENCES customers(customer_id),
  order_date DATE,
  total_amount DECIMAL(10,2),
  status VARCHAR(20)
);
```

## Current Query

```sql
SELECT 
  c.customer_id,
  c.name,
  c.email
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
WHERE EXTRACT(QUARTER FROM o.order_date) = 1
AND EXTRACT(YEAR FROM o.order_date) = 2023
ORDER BY o.total_amount DESC
LIMIT 5;
```

## Expected Result

A table with columns: customer_id, name, email, total_spend
Where total_spend is the sum of all orders for each customer during Q1 2023.
Top 5 customers by that total spend.
```

## Request to AI Assistant

"Can you help me fix my Snowflake SQL query using the context provided? I think I'm missing something since I need to get the total amount spent by each customer, not just order them by individual order amounts."

## AI Assistant Response

Your query isn't aggregating the total spending per customer. Here's the corrected query:

```sql
SELECT 
  c.customer_id,
  c.name,
  c.email,
  SUM(o.total_amount) AS total_spend
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
WHERE EXTRACT(QUARTER FROM o.order_date) = 1
AND EXTRACT(YEAR FROM o.order_date) = 2023
AND o.status = 'completed'  -- Assuming you only want completed orders
GROUP BY c.customer_id, c.name, c.email
ORDER BY total_spend DESC
LIMIT 5;
```

This query:
1. Joins customers and orders tables
2. Filters for Q1 2023 orders
3. Groups by customer information
4. Sums the total_amount for each customer
5. Orders by that sum descending
6. Limits to top 5 customers

I also added a status filter assuming you only want completed orders, but you can remove that if you want to include all orders regardless of status.

## Key Takeaways

1. The minimal template was sufficient for this straightforward query
2. Including the database schema helped identify the relationship between tables
3. Providing the current query attempt clearly showed the missing aggregation
4. Stating the expected output format helped ensure the solution matched the need 