# Comprehensive Snowflake MCP Template

This template includes all aspects of context needed for complex Snowflake SQL tasks. Use sections relevant to your specific needs.

## 1. QUERY CONTEXT

### Business Context
[Describe the business problem or question this query aims to solve]

### Query Objective
[Clearly state what the query should accomplish]

### User Skill Level
[Beginner, Intermediate, Advanced] - helps calibrate explanations

## 2. DATABASE STRUCTURE

### Schema Definitions
```sql
-- Include relevant tables, columns, and relationships
-- Example:
CREATE TABLE customers (
  customer_id INTEGER PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100),
  signup_date DATE
);

CREATE TABLE orders (
  order_id INTEGER PRIMARY KEY,
  customer_id INTEGER REFERENCES customers(customer_id),
  order_date DATE,
  total_amount DECIMAL(10,2)
) CLUSTER BY (order_date);
```

### Table Statistics
```
-- Approximate row counts, sizes, and distributions for relevant tables
-- Example:
-- customers: ~1M rows, 500MB
-- orders: ~10M rows, 5GB, heavy skew toward recent dates
```

### Constraints and Indexes
```
-- Document any relevant constraints, unique indexes, or clustering keys
-- Example:
-- customers has unique index on email
-- orders is clustered by order_date
```

## 3. CURRENT QUERY

```sql
-- Your current query, even if it's incomplete or has errors
SELECT * FROM customers;
```

## 4. QUERY ISSUES (if applicable)

### Error Messages
```
-- Paste exact error messages
```

### Performance Issues
- Current execution time: [e.g., 45 seconds]
- Target execution time: [e.g., under 10 seconds]
- Query profile metrics: [Paste relevant metrics from QUERY_PROFILE]

### Logic Issues
[Describe any incorrect results or logic problems]

## 5. SAMPLE DATA

```
-- Sample data for relevant tables (first few rows)
-- Example:
-- CUSTOMERS:
-- customer_id | name           | email               | signup_date
-- 1           | John Smith     | john@example.com    | 2021-01-15
-- 2           | Jane Doe       | jane@example.com    | 2021-02-20
-- 3           | Bob Johnson    | bob@example.com     | 2021-03-10
```

## 6. EXPECTED RESULTS

```
-- Example of expected output format and sample results
-- Column names and data types
-- Example rows
```

## 7. ENVIRONMENT DETAILS

- Snowflake Edition: [e.g., Enterprise, Business Critical]
- Warehouse Size: [e.g., Medium, Large]
- Client Tool: [e.g., SnowSQL, Snowflake Web UI, dbt]
- Query Context: [e.g., Stored Procedure, View Definition, Ad-hoc]

## 8. VISUALIZATION NEEDS (if applicable)

- Visualization tool: [e.g., Tableau, Power BI, Looker]
- Chart type preferences: [e.g., Time series, Bar chart, Geo map]
- Key metrics to highlight: [e.g., YoY growth, Regional distribution]

## 9. CONSTRAINTS & REQUIREMENTS

- [List any constraints on the solution, e.g., "Can't modify table structures"]
- [List any specific requirements, e.g., "Must use window functions"]
- [Note any performance requirements]
- [Note any formatting requirements]

## 10. PREVIOUS ATTEMPTS

```sql
-- Include any previous query attempts and why they didn't work
```

---

**Note**: Only fill out sections relevant to your specific query needs. For simpler queries, only the query context, database structure, and current query sections may be sufficient. 