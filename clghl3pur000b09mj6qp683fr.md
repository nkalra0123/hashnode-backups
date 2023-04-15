---
title: "Some very useful SQL commands"
seoDescription: "10 most used sql commands"
datePublished: Sat Apr 15 2023 06:15:33 GMT+0000 (Coordinated Universal Time)
cuid: clghl3pur000b09mj6qp683fr
slug: some-very-useful-sql-commands
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/Y9kOsyoWyaU/upload/830b3f632fc15f257b87c383d9cbf74d.jpeg
tags: mysql, sql

---

Structured Query Language (SQL) is a popular language used for managing relational databases. It is an essential tool for developers, data analysts, and database administrators. There are several SQL commands, but some of them are more frequently used than others. In this blog, we'll discuss the 10 most used SQL commands.

1. SELECT
    
    The SELECT statement is the most commonly used SQL command. It is used to retrieve data from one or more tables in a database. The syntax of the SELECT statement is as follows:
    
    ```sql
    SELECT column1, column2, ... FROM table_name;
    ```
    
    Here, column1, column2, and so on represent the names of the columns you want to retrieve from the table, and table\_name is the name of the table you want to query.
    
2. INSERT
    
    The INSERT statement is used to insert new data into a table. The syntax of the INSERT statement is as follows:
    
    ```sql
    INSERT INTO table_name (column1, column2, ...) VALUES (value1, value2, ...);
    ```
    
    Here, table\_name is the name of the table into which you want to insert data, and column1, column2, and so on represent the names of the columns in the table. The values you want to insert into each column are specified in the VALUES clause.
    
3. UPDATE
    
    The UPDATE statement is used to modify existing data in a table. The syntax of the UPDATE statement is as follows:
    
    ```sql
    UPDATE table_name SET column1 = value1, column2 = value2, ... WHERE condition;
    ```
    
    Here, table\_name is the name of the table you want to update, column1, column2, and so on represent the names of the columns you want to update, and value1, value2, and so on represent the new values you want to assign to those columns. The WHERE clause is optional and is used to specify the condition that must be met for the update to take place.
    
4. DELETE
    
    The DELETE statement is used to delete data from a table. The syntax of the DELETE statement is as follows:
    
    ```sql
    DELETE FROM table_name WHERE condition;
    ```
    
    Here, table\_name is the name of the table from which you want to delete data. The WHERE clause is optional and is used to specify the condition that must be met for the delete to take place.
    
5. CREATE TABLE
    
    The CREATE TABLE statement is used to create a new table in a database. The syntax of the CREATE TABLE statement is as follows:
    
    ```sql
    CREATE TABLE table_name (column1 datatype, column2 datatype, ...);
    ```
    
    Here, table\_name is the name of the table you want to create, and column1, column2, and so on represent the names of the columns in the table and their respective data types.
    
6. ALTER TABLE
    
    The ALTER TABLE statement is used to modify an existing table in a database. The syntax of the ALTER TABLE statement is as follows:
    
    ```sql
    ALTER TABLE table_name ADD column_name datatype;
    ```
    
    Here, table\_name is the name of the table you want to modify, and column\_name and datatype represent the name and data type of the new column you want to add to the table.
    
7. DROP TABLE
    
    The DROP TABLE statement is used to delete a table from a database. The syntax of the DROP TABLE statement is as follows:
    
    ```sql
    DROP TABLE table_name;
    ```
    
    Here, table\_name is the name of the table you want to delete.
    
8. CREATE INDEX
    
    The CREATE INDEX statement is used to create an index on a table in a database. The syntax of the CREATE INDEX statement is as follows:
    
    ```sql
    CREATE INDEX index_name ON table_name (column1, column2, ...);
    ```
    
    Here, index\_name is the name you want to give to the index
    
9. COUNT
    
    The COUNT function is used to count the number of rows that meet a specific condition in a table. The syntax of the COUNT function is as follows:
    
    ```sql
    SELECT COUNT(*) FROM table_name;
    ```
    
    Here, table\_name is the name of the table you want to count the rows in. The asterisk (\*) indicates that all rows in the table should be counted. You can also specify a specific column instead of using the asterisk to count the number of non-null values in that column. For example:
    
    ```sql
    SELECT COUNT(column_name) FROM table_name;
    ```
    
    This will count the number of non-null values in the specified column.
    
10. DISTINCT
    
    The DISTINCT keyword is used to retrieve unique values from a column in a table. The syntax of the DISTINCT keyword is as follows:
    
    ```sql
    SELECT COUNT(DISTINCT column_name) FROM table_name;
    ```
    
    Here, column\_name is the name of the column you want to count the distinct values of, and table\_name is the name of the table the column belongs to. This command will return the number of unique values in the specified column.
    

If you liked this blog, [**you can follow me on twitter**](https://twitter.com/nkalra0123), and learn something new with me.