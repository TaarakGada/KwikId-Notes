# ClickHouse

## What is it?
- ClickHouse is an **open-source columnar database** designed for **analytical queries (OLAP)**
- Extremely fast at reading and aggregating large amounts of data
- Think of it as: "the database that can query a billion rows in seconds"
- Made by Yandex, used by Uber, Cloudflare, and many others

## Columnar vs Row-based DB
| | Traditional DB (Postgres) | ClickHouse |
|--|--------------------------|------------|
| Storage | Row by row | Column by column |
| Best for | Transactions (OLTP) | Analytics (OLAP) |
| Read all columns | Fast | Slower (reads more) |
| Aggregate one column | Slow | Very fast (only reads that column) |

## When to Use ClickHouse
- Log analysis (billions of log lines)
- Real-time analytics dashboards
- Time-series data (metrics, events)
- BI reporting on huge datasets
- When Postgres is too slow for analytics

## Key Features
- **Columnar storage**: Only reads columns needed for query
- **Compression**: Columns compress much better than rows
- **Vectorized execution**: Processes batches of data efficiently
- **SQL interface**: Familiar SQL syntax
- **Real-time ingestion**: Can handle millions of inserts/second

## Basic SQL Example
```sql
-- Create a table
CREATE TABLE events (
    event_date Date,
    user_id UInt64,
    event_type String,
    value Float64
) ENGINE = MergeTree()
ORDER BY (event_date, user_id);

-- Insert data
INSERT INTO events VALUES ('2024-01-01', 123, 'click', 1.0);

-- Fast aggregation on millions of rows
SELECT
    event_type,
    count() as total,
    avg(value) as avg_value
FROM events
WHERE event_date >= '2024-01-01'
GROUP BY event_type
ORDER BY total DESC;
```

## Python Client
```python
from clickhouse_driver import Client

client = Client('localhost')

result = client.execute(
    'SELECT count() FROM events WHERE event_type = %(type)s',
    {'type': 'click'}
)
print(result)  # [(42000000,)]
```

## Good to Know
- Not great for frequent updates/deletes (it's append-optimized)
- `MergeTree` engine is the most used — supports partitioning and ordering
- ClickHouse Cloud offers a managed version
- Often paired with **Metabase** for dashboards
- Data is eventually consistent — use `FINAL` modifier if you need latest data
