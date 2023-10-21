---
title: "Unveiling the Power of Generated Columns in MySQL: A Shield Against Duplicate Emails"
seoTitle: "Unlock Data Harmony with MySQL Generated Columns: A Guide to Enhancing"
seoDescription: "Dive into the world of MySQL Generated Columns and discover a powerful tool for data optimization. Learn how to safeguard against duplicate email entries"
datePublished: Sat Oct 21 2023 11:10:30 GMT+0000 (Coordinated Universal Time)
cuid: clnzxv0e7000909jugpsgbm8y
slug: unveiling-the-power-of-generated-columns-in-mysql-a-shield-against-duplicate-emails
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/fPxOowbR6ls/upload/47db404638e723d659e861f565941af0.jpeg
tags: mysql, hacking

---

In this blog post, we'll delve into the workings of generated columns and showcase a practical example where they can be harnessed to prevent users from signing up with subtly different, yet essentially identical, email addresses.

### **Understanding Generated Columns**

Generated columns in MySQL are virtual columns whose values are determined by an expression rather than being directly stored in the database. These columns can be computed based on other columns in the same table, constants, or functions.

To create a generated column, you specify a generation expression when defining the column. This expression can involve mathematical operations, string manipulations, or even date functions. The column's value is then computed automatically, saving the need for manual intervention.

### **Example Scenario: Unifying Email Addresses**

Consider a scenario where you want to ensure that users cannot sign up with email addresses that are technically the same but appear different due to the use of the '+' symbol and additional strings. For instance, [`user@gmail.com`](mailto:user@gmail.com), [`user+123@gmail.com`](mailto:user+123@gmail.com), and [`user+test@gmail.com`](mailto:user+test@gmail.com) should all be treated as the same email address.

### **Creating the Users Table**

Let's create a simplified 'users' table to illustrate this:

```sql
CREATE TABLE users_signup (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(255) NOT NULL,
    normalized_email VARCHAR(255) GENERATED ALWAYS AS (
        CONCAT_WS(
            '@',
            SUBSTRING_INDEX(SUBSTRING_INDEX(email, '@', 1), '+', 1),
            SUBSTRING_INDEX(email, '@', -1)
        )
    ) STORED,
    UNIQUE KEY idx_normalized_email (normalized_email)
);
```

Lets understand this create table command

In MySQL, the `STORED` keyword in the context of a generated column specifies that the values for that column will be stored physically on disk. When you define a generated column, you have the option to store the computed values on disk or to make them virtual (computed on-the-fly when queried).

Here's a breakdown:

* **VIRTUAL**: This is the default behavior. The value of a virtual column is not physically stored on disk; instead, it is computed when queried. Virtual columns are useful when you want to derive a value based on other columns without consuming additional storage.
    
* **STORED**: This option indicates that the computed values for the generated column will be stored on disk. Storing the values can be beneficial if you frequently query or index the generated column, as it avoids the need to recompute the value each time it is accessed.
    

Here we are generating a vitual column, storing that on disk, and the value is determined by this function

```sql
 CONCAT_WS(
            '@',
             SUBSTRING_INDEX(SUBSTRING_INDEX(email, '@', 1), '+', 1),
              SUBSTRING_INDEX(email, '@', -1)
             )
```

We are getting a substring before `'@'` then taking substring of that result before `'+'`

and concating it with `'@'` and substring after `'@'` in the original input.

So now when you try to insert values like this, you will get an error

```sql

INSERT INTO users_signup (email) VALUES ('user+123@gmail.com');

-- This should fail because 'user+test@gmail.com' normalizes to 'user@gmail.com'
INSERT INTO users_signup (email) VALUES ('user+test@gmail.com');

-- This should fail because 'uSer@gmail.com' normalizes to 'user@gmail.com' (case-insensitive)
INSERT INTO users_signup (email) VALUES ('uSer@gmail.com');
```

And only one entry is present in database.

```sql
mysql> select * from users_signup;
+---------+--------------------+------------------+
| user_id | email              | normalized_email |
+---------+--------------------+------------------+
|       1 | user+123@gmail.com | user@gmail.com   |
+---------+--------------------+------------------+
1 row in set (0.00 sec)
```

This way you can stop `"smart"` people trying to hack the free tier of your application.

If you liked this blog, [**you can follow me on twitter**](https://twitter.com/nkalra0123), and learn something new with me.