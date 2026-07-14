# Metabase

## What is it?
- Metabase is an **open-source business intelligence (BI) tool**
- Lets you create charts, dashboards, and reports from your database — **no SQL required** (but SQL is supported)
- Non-technical team members can query data through a visual interface
- Think of it as: a friendly UI sitting on top of your database

## What Can You Do?
- Connect to databases (Postgres, MySQL, ClickHouse, MongoDB, etc.)
- Build charts (bar, line, pie, maps) without writing SQL
- Create dashboards combining multiple charts
- Set up automatic email/Slack reports
- Write custom SQL for advanced queries
- Share dashboards via link (no login required for public)

## Supported Databases
- PostgreSQL, MySQL, SQLite
- ClickHouse (via plugin)
- MongoDB
- BigQuery, Snowflake, Redshift
- Google Analytics

## Quick Setup with Docker
```bash
docker run -d -p 3000:3000 \
  --name metabase \
  -v ~/metabase-data:/metabase-data \
  -e MB_DB_FILE=/metabase-data/metabase.db \
  metabase/metabase

# Open http://localhost:3000
```

## Using SQL in Metabase
```sql
-- You can write native SQL queries
SELECT 
    DATE_TRUNC('day', created_at) as day,
    COUNT(*) as new_users,
    SUM(revenue) as daily_revenue
FROM orders
WHERE created_at >= NOW() - INTERVAL '30 days'
GROUP BY 1
ORDER BY 1;
```

## Key Features
- **Question**: A single query/chart
- **Dashboard**: Collection of Questions arranged on a page
- **Collections**: Folders to organize Questions and Dashboards
- **Permissions**: Control which teams see which data
- **Alerts**: Notify via email/Slack when a metric crosses a threshold

## Good to Know
- Free open-source version covers most needs
- Metabase Cloud is the paid hosted option
- Great combo: **ClickHouse + Metabase** for analytics at scale
- Embeddable — you can embed Metabase charts in your own web app
- Not great for very complex transformations — use dbt + Metabase for that
