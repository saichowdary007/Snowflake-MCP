# Snowflake Performance Optimization MCP Template

Use this template when you need help optimizing the performance of your Snowflake SQL queries.

## Query Context

[Briefly describe what business problem this query is solving and its importance]

## Current Query

```sql
[Paste your complete query that needs optimization]
```

## Database Schema

```sql
-- Include the relevant tables and columns with data types
-- Also include important indexes, clustering keys, etc.
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

-- Table statistics (if known)
-- customers: ~1M rows, 500MB
-- orders: ~10M rows, 5GB
```

## Current Performance

- Execution Time: [e.g., 45 seconds]
- Rows Processed: [e.g., 10 million]
- Bytes Scanned: [e.g., 2GB]
- Warehouse Size: [e.g., Medium]

## Query Profile Insights (Optional)

```
[If available, paste relevant parts of Snowflake's QUERY_PROFILE output]
```

## Performance Goals

[Describe your performance targets, e.g., "Need to reduce execution time to under 10 seconds"]

## Constraints

[List any constraints that must be considered, e.g., "Can't modify the schema", "Need to maintain backward compatibility with existing dashboards"]

## What You've Already Tried

[List any optimization attempts you've already made]

## Environment Details

- Snowflake Edition: [e.g., Enterprise, Business Critical]
- Warehouse Size: [e.g., Medium, Large]
- Compute Resources: [e.g., Standard, Memory-optimized]

---

**Tips for optimization:**
- Include EXPLAIN PLAN output if available
- Note if tables are particularly large or have skewed data
- Mention any specific Snowflake features you're using (micro-partitions, clustering, etc.)
- Specify if certain joins or operations are particularly slow 