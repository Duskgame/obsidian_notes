# Database Partitioning

[PostgreSQL Partitioning Docs](https://www.postgresql.org/docs/current/ddl-partitioning.html) | [AWS DynamoDB Partitions](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.Partitions.html)

Database partitioning splits a large table across multiple physical storage units (partitions), so queries only need to scan the relevant subset of data rather than the entire table.

---

## The Problem

A table with 100 million rows is slow to query even with indexes, because:
- Full table scans read everything
- A single storage unit becomes a bottleneck
- Maintenance operations (vacuum, backup) affect the entire table

Partitioning divides the data so queries can target only the relevant partition.

---

## Partitioning Strategies

### Range Partitioning
Rows are distributed based on a value range of a column. Most common for time-series data.

```sql
CREATE TABLE answers (
    id BIGINT,
    user_id INT,
    answered_at DATE
) PARTITION BY RANGE (answered_at);

CREATE TABLE answers_2026_05 PARTITION OF answers
    FOR VALUES FROM ('2026-05-01') TO ('2026-06-01');
```

Query for `WHERE answered_at = '2026-05-21'` → only scans the May 2026 partition.

### Hash Partitioning
A hash function is applied to a column value. The result determines the partition. Distributes rows evenly.

```sql
PARTITION BY HASH (user_id)
-- user_id % 4 → 4 partitions, rows spread evenly
```

Good for distributing load when there is no natural range.

### List Partitioning
Rows are distributed based on explicit discrete values.

```sql
PARTITION BY LIST (country)
-- 'DE' → partition_eu, 'US' → partition_na
```

---

## Cross-Partition Queries

A query that cannot be resolved by a single partition must scan **all partitions** — a cross-partition query (also called a **full scan** in NoSQL databases).

```sql
-- Single-partition query (fast) — partition key used
SELECT * FROM answers WHERE answered_at = '2026-05-21';

-- Cross-partition query (slow) — partition key not used
SELECT * FROM answers WHERE score > 90;
```

**In cloud NoSQL databases (e.g. DynamoDB):**
- Cross-partition queries are called **Scans**
- Billed per row read, even rows that are later discarded
- Should be avoided by designing data access patterns around the partition key

---

## Partition Key Design

The partition key is the column used to determine which partition a row belongs to. Choosing it poorly leads to:

**Hot partition:** One partition receives most reads/writes while others are idle — defeats the purpose of partitioning.

```
Bad: partition by is_active (only 2 values → 2 partitions, one hugely popular)
Good: partition by user_id hash (spread evenly across N partitions)
```

The rule: the partition key should have **high cardinality** and distribute access **evenly**.

---

## Partitioning in Cloud Databases

| Database | Partitioning mechanism |
|---|---|
| PostgreSQL | Manual range/hash/list partitioning |
| AWS DynamoDB | Automatic; you define partition key + optional sort key |
| Google Bigtable | Row key prefix determines shard |
| Apache Cassandra | Partition key in the primary key; automatic distribution |

In managed NoSQL databases, partitioning is automatic and transparent — but your **data model and key design** directly determine performance.

---

## Partitioning vs Sharding

| Partitioning | Sharding |
|---|---|
| Splits data within one database instance | Splits data across multiple independent database instances |
| Managed by the database engine | Often managed at application level |
| Transparent to most queries | Queries must route to the correct shard |

Sharding is effectively horizontal partitioning across separate nodes.

---

## Related Topics

- [[postgres]] — PostgreSQL supports declarative table partitioning natively
- [[Cloud Native]] — managed cloud databases handle partitioning automatically
- [[Observability]] — partition-level metrics (hot partition alerts) are important in production
