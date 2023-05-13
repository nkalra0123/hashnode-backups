---
title: "MySQL  Partitioning"
datePublished: Sat May 13 2023 12:41:21 GMT+0000 (Coordinated Universal Time)
cuid: clhlz7pn5000k09kw4zwcd07f
slug: mysql-partitioning
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/Y9kOsyoWyaU/upload/60f7d7bfa2a5139620fea423d25c1b31.jpeg
tags: mysql, partition

---

In MySQL, partitioning is a technique used to divide large database tables into smaller, more manageable pieces called partitions. Each partition is essentially an independent "sub-table" with its own storage engine and set of indexes.

Partitioning offers several benefits, including improved performance and manageability. Here are some key points to understand about partitioning in MySQL on a single database node:

1. Data Distribution: Partitioning allows you to distribute your data across multiple physical or logical storage devices, such as different hard drives or file systems. This can enhance I/O performance by spreading the data access workload.
    
2. Query Performance: Partitioning can significantly improve query performance, especially when dealing with large tables. By dividing the data into smaller partitions, the database engine can limit the amount of data it needs to scan or index, resulting in faster query execution.
    
3. Pruning: Partitioning enables the database to "prune" unnecessary partitions when executing queries based on specific conditions. This means that the query optimizer can skip scanning partitions that don't meet the search criteria, saving processing time and resources.
    
4. Maintenance Operations: Partitioning simplifies certain maintenance operations, such as archiving or deleting old data. Instead of deleting rows individually, you can drop or truncate entire partitions, which is more efficient.
    

### **Types of MySQL Partitioning:**

MySQL offers several partitioning methods, each suitable for specific scenarios:

1. **Range Partitioning**: This method divides data based on a specified range of values. For example, a table of sales data could be partitioned by date, with each partition containing data for a specific time period (e.g., a month or a year).
    
2. **List Partitioning**: In this method, data is partitioned based on discrete values specified in a list. For instance, a table containing customer data could be partitioned based on customer types, such as "premium," "regular," and "guest."
    
3. **Hash Partitioning**: Here, the partitioning function assigns data to partitions based on a hash value. Hash partitioning is useful when you want to distribute data evenly across partitions, regardless of the actual values.
    
4. **Key Partitioning**: This method partitions data based on a defined key or column value. It is particularly useful for range-based queries.
    

```sql
CREATE TABLE orders (
    order_id INT,
    order_date DATE,
    customer_id INT,
    PRIMARY KEY (order_id,order_date)
)
PARTITION BY RANGE (YEAR(order_date)) (
    PARTITION p0 VALUES LESS THAN (2022),
    PARTITION p1 VALUES LESS THAN (2023),
    PARTITION p2 VALUES LESS THAN (2024)
);
```

In this example, we created a table with 3 partitions.

```sql
EXPLAIN SELECT * 
FROM orders
WHERE order_date >= '2022-02-01' AND customer_id = 123;
```

Now this query searches only in two partitions p1,p2

You can also use sub-partitioning inside a partition.

You have to keep a few things in mind when your table has PRIMARY or UNIQUE Keys

> The rule governing the relationship between partitioning keys, primary keys, and unique keys is that all columns used in the partitioning expression for a partitioned table must be part of every unique key that the table may have.

Otherwise, you will get one of these errors:

Error Code: 1503. A UNIQUE INDEX must include all columns in the table's partitioning function

Error Code: 1503. A PRIMARY KEY must include all columns in the table's partitioning function

**A partitioning key** is a column or set of columns that is used to divide a table into smaller partitions. For example, a table could be partitioned by date, so that each partition contains data for a different month.

**A primary key** is a column or set of columns that uniquely identifies each row in a table.

**A unique key** is a column or set of columns that cannot contain duplicate values.

In other words, every unique key on the table must use every column in the table's partitioning expression. For example, if a table is partitioned by date, then the primary key must also include the `date` column.

For example, each of the following table creation statements is invalid:

```sql

CREATE TABLE t1 (
    col1 INT NOT NULL,
    col2 DATE NOT NULL,
    col3 INT NOT NULL,
    col4 INT NOT NULL,
    UNIQUE KEY (col1, col2)
)
PARTITION BY HASH(col3)
PARTITIONS 4;

CREATE TABLE t2 (
    col1 INT NOT NULL,
    col2 DATE NOT NULL,
    col3 INT NOT NULL,
    col4 INT NOT NULL,
    UNIQUE KEY (col1),
    UNIQUE KEY (col3)
)
PARTITION BY HASH(col1 + col3)
PARTITIONS 4;
```

Lets make some changes, and now these queries will work

```sql
CREATE TABLE t1 (
    col1 INT NOT NULL,
    col2 DATE NOT NULL,
    col3 INT NOT NULL,
    col4 INT NOT NULL,
    UNIQUE KEY (col1, col2, col3)
)
PARTITION BY HASH(col3)
PARTITIONS 4;

CREATE TABLE t2 (
    col1 INT NOT NULL,
    col2 DATE NOT NULL,
    col3 INT NOT NULL,
    col4 INT NOT NULL,
    UNIQUE KEY (col1, col3)
)
PARTITION BY HASH(col1 + col3)
PARTITIONS 4;
```

Best Practices for MySQL Partitioning:

1. Choose the right partitioning method: Select the partitioning method that aligns with your data characteristics and query patterns. Consider factors like data distribution, expected growth, and types of queries you frequently perform.
    
2. Define an appropriate number of partitions: Too few partitions may limit the benefits of partitioning, while too many can introduce unnecessary overhead. Find the right balance by considering factors such as data size, query performance, and administrative overhead.
    
3. Regularly maintain and optimize partitions: Periodically analyze the partitioned table's performance and make adjustments as needed. Perform regular maintenance tasks like partition pruning, data archival, and optimizing partitioning column indexes.
    

If you liked this blog, [**you can follow me on twitter**](https://twitter.com/nkalra0123), and learn something new with me.