# Databricks SQL Cheat Sheet

## 🏗️ Database & Table Management

### Create database/schema
```sql
CREATE SCHEMA my_db;
```

### List databases
```sql
SHOW SCHEMAS;
```

### Drop database
```sql
DROP SCHEMA my_db CASCADE;
```

### Create managed table
```sql
CREATE TABLE sales (id INT, amount DECIMAL, date DATE);
```

### Create external table
```sql
CREATE TABLE sales_ext
USING delta
LOCATION '/mnt/data/sales';
```

### Drop table
```sql
DROP TABLE sales;
```

### Describe table
```sql
DESCRIBE DETAIL sales;
```

## 📥 Ingestion / Data Loading

### Read raw files into a table (CTAS)
```sql
CREATE TABLE bronze_orders AS
SELECT * 
FROM read_files('/mnt/raw/orders', format => 'json');
```

### Insert data
```sql
INSERT INTO sales VALUES (1, 500.0, '2025-09-15');
```

### Overwrite with insert
```sql
INSERT OVERWRITE TABLE sales
SELECT * FROM new_sales;
```

## 🔄 Delta Table Operations

### MERGE (Upsert)
```sql
MERGE INTO target t
USING source s
ON t.id = s.id
WHEN MATCHED THEN UPDATE SET amount = s.amount
WHEN NOT MATCHED THEN INSERT *;
```

### DELETE
```sql
DELETE FROM sales WHERE date < '2024-01-01';
```

### UPDATE
```sql
UPDATE sales SET amount = amount * 1.1 WHERE date >= '2025-01-01';
```

### Optimize (compaction)
```sql
OPTIMIZE sales;
```

### Z-ordering
```sql
OPTIMIZE sales ZORDER BY (date);
```

### Vacuum (clean old files)
```sql
VACUUM sales RETAIN 168 HOURS;
```

## 📊 Querying & Transformations

### Select
```sql
SELECT id, SUM(amount) 
FROM sales 
WHERE date >= '2025-01-01'
GROUP BY id;
```

### Joins
```sql
SELECT a.id, b.amount
FROM customers a
JOIN sales b ON a.id = b.cust_id;
```

### Window functions
```sql
SELECT id, amount,
    ROW_NUMBER() OVER (PARTITION BY id ORDER BY date DESC) as rn
FROM sales;
```

## 🧰 Utility Commands

### Show tables in schema
```sql
SHOW TABLES IN my_db;
```

### Explain query plan
```sql
EXPLAIN SELECT * FROM sales;
```

### History of Delta table
```sql
DESCRIBE HISTORY sales;
```

### Set current schema
```sql
USE my_db;
```