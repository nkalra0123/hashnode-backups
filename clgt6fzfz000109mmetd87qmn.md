---
title: "Isolation Levels in Databases"
datePublished: Sun Apr 23 2023 08:58:25 GMT+0000 (Coordinated Universal Time)
cuid: clgt6fzfz000109mmetd87qmn
slug: isolation-levels-in-databases
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/Y9kOsyoWyaU/upload/a2374bd41b952aa5d6fe541eacca8bda.jpeg
tags: mysql, databases

---

Isolation levels are a critical aspect of database design and management. They determine how transactions are managed and how they interact with one another, affecting data consistency, concurrency, and performance. Choosing the right isolation level for your database system is essential to ensure that your data is accurate, transactions are completed without errors, and performance is optimized. In this article, we will explore the different isolation levels and their effects on databases.

**What are Isolation Levels?**

Isolation levels define how transactions interact with one another and how they affect the consistency of the database. The four isolation levels defined by the ANSI/ISO SQL standard are:

1. **Read Uncommitted**
    
2. **Read Committed**
    
3. **Repeatable Read**
    
4. **Serializable**
    

Each isolation level has its own set of rules and behaviors that determine how transactions interact and how they are affected by other transactions.

**Read Uncommitted**

Read Uncommitted is the lowest isolation level and allows transactions to read uncommitted changes made by other transactions. This means that transactions can read data that has been changed but not yet committed, which can lead to inconsistencies in the data. This isolation level provides the highest level of concurrency but the lowest level of consistency.

**Read Committed**

Read Committed is a higher isolation level that ensures that transactions only read committed data. This means that transactions will not read data that has been changed but not yet committed. Read Committed provides a better level of consistency than Read Uncommitted but at the cost of lower concurrency.

**Repeatable Read**

Repeatable Read is a higher isolation level that ensures that a transaction sees a consistent snapshot of the database throughout the transaction. This means that if a transaction reads data during the transaction, it will see the same data throughout the entire transaction, even if other transactions modify the data. Repeatable Read provides a higher level of consistency than Read Committed but at the cost of even lower concurrency.

**Serializable**

Serializable is the highest isolation level that ensures that transactions are executed in a completely isolated manner. This means that transactions are executed as if they are the only transactions in the system, preventing any concurrent modifications. Serializable provides the highest level of consistency but at the cost of the lowest concurrency.

### Choosing the Right Isolation Level

Choosing the right isolation level for your database system depends on several factors, including the level of concurrency required, the criticality of the data, and the performance requirements. Read Uncommitted and Read Committed are suitable for systems with high concurrency requirements but low criticality data, while Repeatable Read and Serializable are better suited for systems with high criticality data but lower concurrency requirements.

It's important to note that while higher isolation levels provide better consistency, they also have a higher performance cost due to the need to lock resources for the duration of transactions. It's essential to find a balance between consistency and performance to ensure optimal database performance.

Isolation levels are a critical aspect of database design and management. They determine how transactions interact with one another and how they affect data consistency, concurrency, and performance. Choosing the right isolation level for your database system is essential to ensure that your data is accurate, transactions are completed without errors, and performance is optimized. By understanding the different isolation levels and their effects on databases, you can make informed decisions about which level is best suited for your system's requirements.

Example to reproduce **phantom read in MySQL.**

With the steps mentioned on Wikipedia, you will not be able to reproduce phantom reads in mysql innodb with default isolation level (REPEATABLE READ), but these steps works with READ COMMITTED isolation level

| Transaction 1 | Transaction 2 |
| --- | --- |
| **BEGIN**; **SELECT** name **FROM** users **WHERE** age &gt; 17; *\-- retrieves Alice and Bob* |  |
|  | **BEGIN**; **INSERT** **INTO** users **VALUES** (3, 'Carol', 26); **COMMIT**; |
| **SELECT** name **FROM** users **WHERE** age &gt; 17; *\-- READ UNCOMMITTED retrieves Alice, Bob and Carol (phantom read)* *\-- READ COMMITTED retrieves Alice, Bob and Carol (phantom read)* *\-- REPEATABLE READ retrieves Alice, Bob and Carol (phantom read)* *\-- SERIALIZABLE retrieves Alice and Bob (phantom read has been avoided)* **COMMIT**; |  |

You can produce **phantom read** in `REPEATABLE READ` in **MySQL**.

First, set `REPEATABLE READ`:

```sql
SET GLOBAL TRANSACTION ISOLATION LEVEL REPEATABLE READ;
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

Then, create `person` table with `id` and `name` as shown below.

### `person` table:

| **id** | **name** |
| --- | --- |
| **1** | **John** |
| **2** | **David** |

Then, take **these steps below** with **MySQL queries**. \*I used **2 command prompts**:

| **Flow** | **Transaction 1 (T1)** | **Transaction 2 (T2)** | **Explanation** |
| --- | --- | --- | --- |
| **Step 1** | `BEGIN;` |  | **T1 starts.** |
| **Step 2** |  | `BEGIN;` | **T2 starts.** |
| **Step 3** | `SELECT * FROM person;`  
  
**1 John**  
**2 David** |  | **T1 reads 2 rows.** |
| **Step 4** |  | `INSERT INTO person VALUES (3, 'Tom');` | **T2 inserts the row with** `3` and `Tom` to `person` table. |
| **Step 5** |  | `COMMIT;` | **T2 commits.** |
| **Step 6** | `SELECT * FROM person;`  
  
**1 John**  
**2 David** |  | **T1 reads 2 rows after T2 commits.**  
  
Phantom read doesn't occur for now!! |
| **Step 7** | `UPDATE person set name = 'Lisa' where id = 3;` |  | **Now to your surprise, T1 can update the new row which T2 has just inserted from** `Tom` to `Lisa`. |
| **Step 8** | `SELECT * FROM person;`  
  
**1 John**  
**2 David**  
**3 Lisa** |  | **Now to your surprise, T1 reads 3 rows after T2 commits.**  
  
Phantom read occurs!! |
| **Step 7** | `COMMIT;` |  | **T1 commits.** |

In addition, I did these steps above in `REPEATABLE READ` in **Postgresql** but **phantom read** didn't occur.

These steps are from a StackOverflow [answer](https://stackoverflow.com/questions/5444915/how-to-produce-phantom-read-in-repeatable-read-mysql/41178461#41178461)

To check the current isolation level you can use this command.

```sql
SELECT @@tx_isolation;
```

If you liked this blog, [**you can follow me on twitter**](https://twitter.com/nkalra0123), and learn something new with me.