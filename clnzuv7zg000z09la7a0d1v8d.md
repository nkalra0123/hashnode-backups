---
title: "Storing emojis in MySQL table"
seoTitle: "Mastering MySQL for Emojis: A Guide to utf8mb4, Collations"
seoDescription: "Unlock the Power of Emojis in MySQL: A Guide to Storing and Retrieving Emotive Characters. Dive into the intricacies of character sets and collations"
datePublished: Sat Oct 21 2023 09:46:41 GMT+0000 (Coordinated Universal Time)
cuid: clnzuv7zg000z09la7a0d1v8d
slug: storing-emojis-in-mysql-table
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/CUCGsuAyufc/upload/dc2dfd78930fa67811961d5b85b01663.jpeg
tags: mysql, databases, utf8, utf-8mb4

---

In the ever-evolving landscape of web development, the need to support diverse and expressive content, such as emojis, has become paramount. Emojis bring a new dimension to communication, and incorporating them into your MySQL database requires careful consideration of character sets and collations.

### What is UTF-8

UTF-8 is a variable-width character encoding that can represent every character in the Unicode character set. It assigns a unique code point to each character, including emojis. Emojis are represented by code points beyond the Basic Multilingual Plane (BMP), and UTF-8 supports these extended code points.

### **Character Sets:**

A character set is a collection of characters with a specific encoding. It defines how individual characters are represented as binary data. In MySQL, common character sets include:

1. **latin1:** Supports Western European languages.
    
2. **utf8:** Supports a wide range of characters, but not emojis or characters outside the Basic Multilingual Plane (BMP).
    
3. **utf8mb4:** Supports the entire Unicode character set, including emojis and characters outside the BMP.
    

If you have ever encountered this error, this means you tried to save some characters in a column, that doesn't support saving these characters.

> Caused by: java.sql.SQLException: Incorrect string value: '\\xE2\\x80\\x8D\\xEF\\xB8\\x8F...' for column 'comment' at row 1

### Character Set: utf8mb4

MySQL's utf8mb4 character set is essential for storing emojis, as it supports a wider range of characters compared to the traditional utf8. Emojis are encoded using more than three bytes, and utf8mb4 allows for this extended character encoding.

When creating a table, set the character set to utf8mb4 for columns that will store emojis:

```sql
CREATE TABLE my_table (
    emoji_column VARCHAR(255) CHARACTER SET utf8mb4,
    -- other columns
);
```

<mark>Emojis are UTF8 chars, but mysql </mark> **<mark>utf8</mark>** <mark> charset doesn't have full support for emojis, so you need utf8mb4</mark>

### COLLATE: utf8mb4\_unicode\_ci

Collation refers to the rules used for comparing and sorting characters in a character set. For utf8mb4, using the collation `utf8mb4_unicode_ci` is recommended. Let's break down its components:

* **utf8mb4**: Specifies the character set.
    
* **unicode**: Indicates that the collation follows Unicode sorting rules.
    
* **ci (case-insensitive)**: Denotes that the comparison is case-insensitive.
    

### **Understanding COLLATE Options:**

MySQL supports various collations, each serving different linguistic and case sensitivity needs. Here's a brief overview:

#### a. Case Sensitivity:

* ***ci (case-insensitive)***: Ignores case differences in comparisons.
    
* ***cs (case-sensitive)***: Considers case differences in comparisons.
    

#### b. Accent Sensitivity:

* ***ai (accent-insensitive)***: Ignores accent differences in comparisons.
    
* ***as (accent-sensitive)***: Considers accent differences in comparisons.
    

You can run `SHOW CHARACTER SET` to see the list of character sets supported

```sql
mysql> SHOW CHARACTER SET;
+----------+---------------------------------+---------------------+--------+
| Charset  | Description                     | Default collation   | Maxlen |
+----------+---------------------------------+---------------------+--------+
| big5     | Big5 Traditional Chinese        | big5_chinese_ci     |      2 |
| dec8     | DEC West European               | dec8_swedish_ci     |      1 |
| cp850    | DOS West European               | cp850_general_ci    |      1 |
| hp8      | HP West European                | hp8_english_ci      |      1 |
| koi8r    | KOI8-R Relcom Russian           | koi8r_general_ci    |      1 |
| latin1   | cp1252 West European            | latin1_swedish_ci   |      1 |
| utf8mb4  | UTF-8 Unicode                   | utf8mb4_general_ci  |      4 |
```

You can run `SHOW COLLATION LIKE 'utf8mb4%'` to see the list of collations available.

```sql
mysql> SHOW COLLATION LIKE 'utf8mb4%';
+------------------------+---------+-----+---------+----------+---------+
| Collation              | Charset | Id  | Default | Compiled | Sortlen |
+------------------------+---------+-----+---------+----------+---------+
| utf8mb4_general_ci     | utf8mb4 |  45 | Yes     | Yes      |       1 |
| utf8mb4_bin            | utf8mb4 |  46 |         | Yes      |       1 |
| utf8mb4_unicode_ci     | utf8mb4 | 224 |         | Yes      |       8 |
| utf8mb4_icelandic_ci   | utf8mb4 | 225 |         | Yes      |       8 |
| utf8mb4_latvian_ci     | utf8mb4 | 226 |         | Yes      |       8 |
| utf8mb4_romanian_ci    | utf8mb4 | 227 |         | Yes      |       8 |
```

The main difference between `utf8mb4_general_ci` and `utf8mb4_unicode_ci` lies in the way they handle sorting and comparison of characters, particularly when it comes to non-European languages and characters with diacritics (accents). Let's explore each collation:

* **Performance vs. Precision:**
    
    * If you prioritize performance and are working with a diverse range of data but don't need language-specific sorting, `utf8mb4_general_ci` might be more suitable.
        
* **Accurate Language Sorting:**
    
    * If you need more accurate language-specific sorting, especially for non-European languages, or if you're dealing with complex linguistic requirements, `utf8mb4_unicode_ci` is a better choice.
        
* General collations use simpler sorting rules, which may result in faster performance for certain operations.
    

```sql
CREATE TABLE example_table (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);
```

When we don't specify the charset and collation, mysql applies the default values, you can check with `show create table` command

```sql
mysql> show create table example_table;
+---------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table         | Create Table                                                                                                                                           |
+---------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| example_table | CREATE TABLE `example_table` (
  `id` int(11) NOT NULL,
  `name` varchar(50) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1 |
+---------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)

mysql> SHOW COLLATION LIKE 'latin1%';
+-------------------+---------+----+---------+----------+---------+
| Collation         | Charset | Id | Default | Compiled | Sortlen |
+-------------------+---------+----+---------+----------+---------+
| latin1_german1_ci | latin1  |  5 |         | Yes      |       1 |
| latin1_swedish_ci | latin1  |  8 | Yes     | Yes      |       1 |
| latin1_danish_ci  | latin1  | 15 |         | Yes      |       1 |
| latin1_german2_ci | latin1  | 31 |         | Yes      |       2 |
| latin1_bin        | latin1  | 47 |         | Yes      |       1 |
| latin1_general_ci | latin1  | 48 |         | Yes      |       1 |
| latin1_general_cs | latin1  | 49 |         | Yes      |       1 |
| latin1_spanish_ci | latin1  | 94 |         | Yes      |       1 |
+-------------------+---------+----+---------+----------+---------+
8 rows in set (0.00 sec)
```

Let's see the case sensitive and insensitive with an example.

First, we will create a table, We then insert some data into the table, including names with varying cases. Finally, we perform a case-insensitive comparison in the `SELECT` statement, searching for rows where the name is equal to 'APPLE'. The result will include the row with 'apple' because the comparison is insensitive to the case differences.

```sql
-- Create a table with a case-insensitive collation
CREATE TABLE example_table (
    id INT PRIMARY KEY,
    name VARCHAR(50) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci
);

-- Insert some data into the table
INSERT INTO example_table (id, name) VALUES
(1, 'apple'),
(2, 'Banana'),
(3, 'Orange'),
(4, 'cherry');

-- Perform case-insensitive comparisons
SELECT * FROM example_table WHERE name = 'APPLE';
+----+-------+
| id | name  |
+----+-------+
|  1 | apple |
+----+-------+
```

Now we drop the table and create a new one with case-sensitive collation.

```sql
CREATE TABLE example_table (
    id INT PRIMARY KEY,
        name VARCHAR(50) CHARACTER SET latin1 COLLATE latin1_general_cs
);
-- Insert some data into the table
INSERT INTO example_table (id, name) VALUES
(1, 'apple'),
(2, 'Banana'),
(3, 'Orange'),
(4, 'cherry');

-- Perform case-insensitive comparisons
SELECT * FROM example_table WHERE name = 'APPLE';

Empty set (0.01 sec)
```

In this example we don't get any result when we search for `APPLE`, but we get the results only when we search for `apple`

```sql
SELECT * FROM example_table WHERE name = 'apple';
+----+-------+
| id | name  |
+----+-------+
|  1 | apple |
+----+-------+
1 row in set (0.01 sec)
```

If you liked this blog, [**you can follow me on twitter**](https://twitter.com/nkalra0123), and learn something new with me.