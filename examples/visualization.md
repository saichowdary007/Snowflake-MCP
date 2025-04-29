# Visualization Example Using MCP Template

## Problem Statement

A marketing analyst needs help creating effective visualizations from a Snowflake query that analyzes customer acquisition channels and their performance over time.

## Filled Template

```markdown
# Snowflake Visualization & Dashboard MCP Template

## Visualization Goal

I need to create a dashboard that shows our customer acquisition performance by marketing channel over the past 12 months. The goal is to identify which channels are most effective in terms of customer acquisition cost (CAC), customer lifetime value (CLV), and retention rate, so we can optimize our marketing budget allocation.

## Target Audience

The primary audience is the marketing leadership team, who have a good understanding of marketing metrics but aren't technical SQL users. They need to make strategic decisions about channel investments for the upcoming fiscal year planning.

## Query Producing the Data

```sql
WITH customer_acquisition AS (
    SELECT 
        DATE_TRUNC('month', c.signup_date) AS month,
        c.acquisition_channel,
        COUNT(DISTINCT c.customer_id) AS new_customers,
        SUM(m.campaign_spend) AS channel_spend,
        SUM(m.campaign_spend) / COUNT(DISTINCT c.customer_id) AS acquisition_cost
    FROM customers c
    JOIN marketing_spend m ON DATE_TRUNC('month', c.signup_date) = m.month 
                           AND c.acquisition_channel = m.channel
    WHERE c.signup_date >= DATEADD(month, -12, CURRENT_DATE())
    GROUP BY 1, 2
),
customer_value AS (
    SELECT 
        DATE_TRUNC('month', c.signup_date) AS month,
        c.acquisition_channel,
        COUNT(DISTINCT c.customer_id) AS cohort_size,
        COUNT(DISTINCT CASE WHEN DATEDIFF(month, c.signup_date, CURRENT_DATE()) >= 3 
              AND c.status = 'active' THEN c.customer_id END) AS retained_after_3m,
        SUM(o.order_total) AS total_revenue
    FROM customers c
    LEFT JOIN orders o ON c.customer_id = o.customer_id 
                      AND o.order_date <= DATEADD(month, 6, c.signup_date)
    WHERE c.signup_date >= DATEADD(month, -12, CURRENT_DATE())
    GROUP BY 1, 2
)
SELECT 
    ca.month,
    ca.acquisition_channel,
    ca.new_customers,
    ca.channel_spend,
    ca.acquisition_cost,
    cv.retained_after_3m,
    (cv.retained_after_3m / NULLIF(cv.cohort_size, 0)) * 100 AS retention_rate_3m,
    cv.total_revenue,
    cv.total_revenue / NULLIF(ca.new_customers, 0) AS revenue_per_customer,
    (cv.total_revenue / NULLIF(ca.new_customers, 0)) / NULLIF(ca.acquisition_cost, 0) AS roi
FROM customer_acquisition ca
JOIN customer_value cv ON ca.month = cv.month AND ca.acquisition_channel = cv.acquisition_channel
ORDER BY ca.month DESC, roi DESC;
```

## Sample Data Format

```
month        | acquisition_channel | new_customers | channel_spend | acquisition_cost | retained_after_3m | retention_rate_3m | total_revenue | revenue_per_customer | roi
-------------+---------------------+---------------+---------------+------------------+-------------------+-------------------+---------------+----------------------+------
2023-11-01   | Paid Search         | 1245          | 87150.00      | 70.00            | 872               | 70.04             | 124500.00     | 100.00               | 1.43
2023-11-01   | Social Media        | 2100          | 105000.00     | 50.00            | 1365              | 65.00             | 168000.00     | 80.00                | 1.60
2023-11-01   | Referral            | 875           | 26250.00      | 30.00            | 700               | 80.00             | 113750.00     | 130.00               | 4.33
2023-10-01   | Paid Search         | 1100          | 77000.00      | 70.00            | 770               | 70.00             | 110000.00     | 100.00               | 1.43
```

## Visualization Type Preferences

I'd like to create:
1. A time series dashboard showing trends over the 12-month period
2. Comparative visualizations that make it easy to see which channels perform best
3. Some way to visualize the ROI calculation that makes it intuitive

## Key Metrics and Dimensions

- Primary Metrics: Acquisition cost, Retention rate, Revenue per customer, ROI
- Dimensions: Month, Acquisition channel
- Filters: Time period (ability to select specific months), Channel (ability to include/exclude channels)

## Tool Information

- Visualization Tool: Tableau
- Integration Method: Direct Snowflake Connection
- Technical Constraints: Dashboard must be responsive and work well on desktop and tablet

## Design Requirements

- Use company color scheme (primary colors: #2C3E50, #E74C3C, #3498DB, #2ECC71)
- Include ability to download the underlying data as CSV
- Should be printable for executive meetings

## Interactive Features Needed

- Tooltips with detailed information when hovering over data points
- Filter controls for date ranges and channels
- Ability to drill down into a specific channel's performance
- Month-over-month comparison feature

## Previous Visualization Attempts

I previously created a basic line chart showing acquisition cost over time by channel, but stakeholders found it hard to understand the full picture of channel performance without seeing the other metrics alongside it.
```

## Request to AI Assistant

"I need help designing an effective Tableau dashboard based on my Snowflake query results. I've provided the query, data sample, and visualization requirements in the template. Please suggest the best way to visualize this marketing channel performance data."

## AI Assistant Response

Based on your requirements, I recommend a comprehensive Tableau dashboard with these components:

### 1. Executive Summary Section

**Key Performance Indicator (KPI) Cards**
- Current month's aggregate metrics with month-over-month comparison
- Total new customers, average acquisition cost, average retention rate, and overall ROI
- Use conditional formatting to highlight positive/negative trends

### 2. Channel Performance Overview

**Channel Comparison Bar Chart**
- X-axis: Marketing channels
- Y-axis: ROI (primary metric)
- Bar color: Based on your company color scheme
- Secondary axis: New customers (represented as line)
- Size: Revenue per customer
- This gives executives an immediate view of which channels are most effective

### 3. Trend Analysis Section

**Multi-metric Time Series**
- Create a line chart showing trends over 12 months
- Include toggle buttons to switch between metrics:
  - Acquisition cost
  - Retention rate
  - Revenue per customer
  - ROI
- Allow multi-channel selection for comparison

### 4. ROI Visualization

**Quadrant Scatter Plot**
- X-axis: Acquisition cost
- Y-axis: Revenue per customer
- Size: Number of new customers
- Color: Channel
- Reference line at ROI = 1.0
- This creates an intuitive visualization of ROI (channels in top-left quadrant are best performers)

### 5. Channel Details (Drill-down)

**Channel Performance Cards**
- Create a separate tab or collapsible section for each channel
- Show detailed month-by-month performance
- Include small multiples for each metric
- Add capacity for annotations to explain specific campaign activities

### 6. Retention Analysis

**Cohort Retention Heatmap**
- Display retention rates by acquisition month and channel
- Color intensity represents retention rate
- This helps identify if certain time periods produce better customer quality

### 7. Interactive Elements

- **Global Filters**
  - Date range selector
  - Channel selector
  - Metric threshold filters
- **Tooltips**
  - Enhance with relevant context and calculation explanations
  - Include sparklines for trend context
- **Dashboard Actions**
  - Click channel name to filter all visualizations
  - Click month to see detailed performance for that period

### 8. Technical Implementation

- Structure the dashboard with fixed size (1200×800) with responsive elements
- Create a printer-friendly version with optimized layouts
- Set up direct Snowflake connection with appropriate refresh schedule
- Add export options for underlying data tables
- Implement parameter controls for flexible date ranges

This comprehensive dashboard design addresses your visualization goals while making it easy for marketing leadership to identify top-performing channels and make data-driven budget allocation decisions.

## Key Takeaways

1. The visualization template provided specific context about the audience's technical level and decision-making needs
2. Including the query and sample data format helped ensure visualization recommendations matched the available data
3. Brand requirements and technical constraints informed design choices appropriate for the organization
4. The previous visualization attempt helped identify what was missing in earlier approaches 