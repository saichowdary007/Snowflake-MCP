# Snowflake SQL Debugging MCP Template

Use this template when you need help troubleshooting and fixing errors in your Snowflake SQL queries.

## Query Context

[Briefly describe what you're trying to accomplish with this query]

## Error Message

```
[Paste the exact error message you're receiving]
```

## Query With Error

```sql
[Paste your complete query that's producing the error]
```

## Database Schema

```sql
-- Include the relevant tables and columns
-- Example:
CREATE TABLE customers (
  customer_id INTEGER PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100)
);

CREATE TABLE orders (
  order_id INTEGER PRIMARY KEY,
  customer_id INTEGER REFERENCES customers(customer_id),
  order_date DATE,
  total_amount DECIMAL(10,2)
);
```

## Sample Data

```sql
-- Sample data from relevant tables (if available)
-- Example:
-- First 3 rows of customers
-- customer_id | name           | email
-- 1           | John Smith     | john@example.com
-- 2           | Jane Doe       | jane@example.com
-- 3           | Bob Johnson    | bob@example.com
```

## Previous Attempts

[Describe any troubleshooting steps you've already tried]

## Environment Details (Optional)

- Snowflake Edition: [e.g., Enterprise, Business Critical]
- Client Tool: [e.g., SnowSQL, Snowflake Web UI, dbt]
- Query Context: [e.g., Stored Procedure, View Definition, Ad-hoc]

---

**Tips for effective debugging:**
- Always include the complete, unmodified error message
- Provide the exact query that's generating the error
- Include enough schema details to understand table relationships
- Mention if you're using any Snowflake-specific features like streams, tasks, etc. 