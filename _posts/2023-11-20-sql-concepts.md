---
layout: post
title: SQL Concepts
short-description: One brief introdution about SQL. SQL stands for Structured Query Language, whereas you can access, manipulate...
date: 2023-11-20
---

# SQL Concepts

For me SQL it is always hard to do when things goes beyond the simple queries (i'm not used to execute SQL in my daily work). Whenever I need to create complex queries, I find myself spending a significant amount of time on research. Now, i would like to share one thing that has been helping me a lot when i need to use SQL.

One brief introdution about SQL. SQL stands for Structured Query Language, whereas you can access, manipulate  and interacting databases. In this post i will cover relational SQL (RDBMS - Relational Database Management System).

I will assume that you already have the basic concepts such as Tables, Databases, Field, Row, Column, Index.

SQL has a command line interface, which you can interact with database, there are some types of sql command;

## DDL - Data Definition Language

* Create
* Alter
* Drop
* Rename
* Insert
* Update
* Delete
* Merge
* Truncate

## DML - Data Manipulation Language

* Insert
* Update
* Delete 
* Merge

## DCL - Data Control Language

* Grant
* Revoke

## DQL - Data Query Language

* Select

# SQL Statements Concepts

A SQL statement is a set of instruction that consists of identifiers, parameters, variables, names, data types, and SQL reserved words that compiles successfully.

## SELECT

The SQL SELECT statement returns a result set of records, from one or more tables. A SELECT statement retrieves zero or more rows from one or more database tables or database views.

| **SELECT** | **Description** | 
| === | === |
| ALL | return all columns. |
| DISTINCT | return only distinct (different) values. |
| Count() | returns the number of rows that matches a specified criterion. |
| Count(DISTINCT exp) | function with DISTINCT clause eliminates the repetitive appearance of the same data. | 

## ALIAS

Aliases are the temporary names given to tables or columns for the purpose of a particular SQL query.

| **ALIAS** | **Description** | 
| === | === |
| As | aliases are used to give a table, or a column in a table, a temporary name |

## GROUP BY

SQL GROUP BY statement groups rows that have the same values into summary rows, like "find the number of customers in each country. 

| **GROUP BY** | **Description** | 
| === | === |
| Group by Column | placing all the rows with the same value of only that particular column in one group. |
| Having | it filters data after rows are grouped and values are aggregated. | 

## ORDER BY

The ORDER BY command is used to sort the result set in ascending or descending order. The ORDER BY command sorts the result set in ascending order by default.

| **ORDER BY** | **Description** | 
| === | === |
| Order by (ASC) | sort the data returned in ascending order. |
| Order by DESC | sort the data returned in descending order. |

## FUNCTIONS

SQL functions are pre-written actions that can be called on individual cells, multiple cells in a record or multiple records.

| **FUNCTIONS** | **Description** | 
| === | === |
| AVG() | returns the average value of an expression. |
| SUM() | returns the sum of all values or the sum of only values specified through conditional expressions. | 
| Count() | returns the number of rows that matches a specified criterion. |
| MIN() | returns the smallest value of the selected column. | 
| MAX() | returns the largest value of the selected column. |

## WHERE

The WHERE clause is used to filter records based on a specific condition.

| **WHERE** | **Description** | 
| === | === |
| LIKE | used to determine whether a specific character string matches a specified pattern. | 
| IN | allows you to specify multiple values in a WHERE clause. The IN operator is a shorthand for multiple OR conditions. | 
| BETWEEN | allows you to easily test if an expression is within a range of values (inclusive). | 
| ANY | the condition will be true if the operation is true for any of the values in the range. | 
| Exists | used to test for the existence of any record in a subquery. | 
| AND, OR, NOT | used to filter records based on more than one condition. | 

## JOINS

The SQL JOIN is a command clause that combines records from two or more tables in a database. It is a means of combining data in fields from two tables by using values common to each table.

| **JOINS** | **Description** | 
| === | === |
| Inner Join | returns all rows from tables where the key record of one table is equal to the key records of another table. |
| Left Join | returns all records from the left table (table1), and the matching records from the right table (table2). |
| Right Join | returns all records from the right table (table2), and the matching records from the left table (table1). | 
| Full Join | returns all records when there is a match in left (table1) or right (table2) table records. |  

# Managing SQL

* Differences Between INNER,LEFT,RIGHT and FULL JOIN

![SQL Joins](https://raw.githubusercontent.com/rca0/rca0.github.io/master/_posts/assets/sql-joins.png)

### INNER

```sql
SELECT select_list
FROM TABLE_A A
    INNER JOIN TABLE_B B
        ON A.key = B.key;
```

|   **TABLE_A**       |      ||    **TABLE_B**   |     |
|  ===                | ===  |         | ===     | === |
| **Key** | **Value** |      | **Key** | **Value**     |
| ===     | ===       |      | ===     | ===           |        
| X1      | 1         |      | 1       | Y1            |
| X2      | 2         |      | 2       | Y2            |
| X3      | 3         |      | 3       | Y3            |

* Output

| **Key Y** | **Value X** | **Value Y** |
| === | === | === |
| 1 | X1 | Y1 |
| 2 | X2 | Y2 |

---

### LEFT (OUTER) JOIN

```sql
SELECT select_list
FROM TABLE_A A
    LEFT JOIN TABLE_B B
        ON A.key = B.key;
```

|   **TABLE_A**       |      ||    **TABLE_B**   |     |
|  ===                | ===  |         | ===     | === |
| **Key** | **Value** |      | **Key** | **Value**     |
| ===     | ===       |      | ===     | ===           |        
| X1      | 1         |      | 1       | Y1            |
| X2      | 2         |      | 2       | Y2            |
| X3      | 3         |      | 3       | NULL          |

* Output

| **Key Y** | **Value X** | **Value Y** |
| === | === | === |
| 1 | X1 | Y1   |
| 2 | X2 | Y2   |
| 3 | X2 | NULL |

--- 

### RIGHT (OUTER) JOIN

```sql
SELECT select_list
FROM TABLE_A A
    RIGHT JOIN TABLE_B B
        ON A.key = B.key;
```

|   **TABLE_A**       |      ||    **TABLE_B**   |     |
|  ===                | ===  |         | ===     | === |
| **KEY** | **VALUE** |      | **KEY** | **VALUE**     |
| ===     | ===       |      | ===     | ===           |        
| X1      | 1         |      | 1       | Y1            |
| X2      | 2         |      | 2       | Y2            |
| X3      | 3         |      | 4       | Y3            |
| NULL    |           |      |         |               |

* Output

| **KEY Y** | **VALUE X** | **VALUE Y** |
| === | === | === |
| 1 | X1   | Y1   |
| 2 | X2   | Y2   |
| 4 | NULL | Y3   |

---

### FULL (OUTER) JOIN

```sql
SELECT select_list
FROM TABLE_A A
    FULL OUTER JOIN TABLE_B B
        ON A.key = B.key;
```

|   **TABLE_A**       |      ||    **TABLE_B**   |     |
|  ===                | ===  |         | ===     | === |
| **Key** | **Value** |      | **Key** | **Value**     |
| ===     | ===       |      | ===     | ===           |        
| X1      | 1         |      | 1       | Y1            |
| X2      | 2         |      | 2       | Y2            |
| X3      | 3         |      | 4       | Y3            |
| NULL    |           |      |         | NULL          |

* Output

| **Key Y** | **Value X** | **Value Y** |
| === | === | === |
| 1 | X1   | Y1   |
| 2 | X2   | Y2   |
| 3 | X3   | NULL |
| 4 | NULL | Y3   |

## Managing Tables

* Create new tables with three columns

```sql
CREATE TABLE table_a (
    id INT PRIMARY_KEY,
    name VARCHAR NOT NULL,
    price INT DEFAULT 0,
);
```

* Delete the table from database

```sql
DROP TABLE table_a;
```

* Add a new column to the table

```sql
ALTER TABLE table_name ADD column;
```

* Drop column c from the table

```sql
ALTER TABLE table_name DROP COLUMN c;
```

* Add a constraint

Table constraints allow you to specify more than one column in a PRIMARY KEY, UNIQUE, CHECK, or FOREIGN KEY constraint definition. Column-level constraints (except for check constraints) refer to only one column.

```sql
ALTER TABLE table_name ADD constraint;
```

* Drop a constraint

```sql
ALTER TABLE table_name DROP constraint;
```

* Rename table from t1 to t2

```sql
ALTER TABLE t1 RENAME TO t2;
```

* Rename column c1 to c2

```sql
ALTER TABLE t1 RENAME c1 to c2;
```

* Remove all data in a table

```sql
TRUNCATE TABLE table_name;
```

## Managing Triggers

An SQL trigger allows you to specify SQL actions that should be executed automatically when a specific event occurs in the database. 

For example, you can use a trigger to automatically update a record in one table whenever a record is inserted into another table.

* Create or modify a trigger

```sql
CREATE OR MODIFY TRIGGER trigger_name
WHEN EVENT
ON table_name TRIGGER_TYPE
EXECUTE stored_procedure;
```

## WHEN 
* BEFORE: invoke before the event occurs.
* AFTER: invoke after the event occurs.

### EVENT
* INSERT: invoke for INSERT
* UPDATE: invoke for UPDATE
* DELETE: invoke for DELETE

### TRIGGER_TYPE
* FOR EACH ROW
* FOR EACH STATEMENT


* Create a trigger invoked before a new row is inserted into the person table

```sql
CREATE TRIGGER before_insert_person
BEFORE INSERT
ON person FOR EACH ROW
EXECUTE stored_procedure;
```

* Delete a specific trigger

```sql
DROP TRIGGER trigger_name;
```

## Modyfing Data

* Insert one row into a table

```sql
INSERT INTO t(column_list)
VALUES(value_list);
```

* Insert multiple rows into a table

```sql
INSERT INTO t(column_list)
VALUES (values_list),
       (values_list), ...;
```

* Insert rows from t2 into t1

```sql
INSERT INTO t1(column_list)
SELECT column_list FROM t2;
```

* Update new value in the column c1 for all rows

```sql
UPDATE table_name
SET c1 = new_value;
```

* Update values in column c1, c2 that match the condition

```sql
UPDATE table_name
SET c1 = new_value,
    c2 = new_value
    WHERE condition;
```

* Delete all data in a table

```sql
DELETE FROM table;
```

* Delete subnet of rows in a table

```sql
DELETE FROM table_name
WHERE condition;
```

---

##  Subqueries

A subquery is a query within another query. The outer query is called as main query and inner query is called as subquery.

## Syntax

```
SELECT select_list
FROM table
WHERE exp_operation
     (SELECT select_list
        FROM table
     );
```

## Using SQL Constraints

* Set column 1 (c1) and column 2 (c2) as a primery key

```sql
CREATE TABLE t(
    c1 INT, c2 INT, c3 VARCHAR,
    PRIMARY KEY (c1,c2)
);
```

* Set c2 column as a Foreign Key

```sql
CREATE TABLE t1(
    c1 INT PRIMARY KEY,
    c2 INT,
    FOREIGN KEY (c2) REFERENCES t2(c2)
);
```

* Make the values in c1 and c2 unique

```sql
CREATE TABLE t(
    c1 INT,
    c1 INT
    UNIQUE(c2, c3)
);
```

* Ensure c1 > 0 and values in c1 >= c2

```sql
CREATE TABLE t(
    c1 INT, c2 INT,
    CHECK(c1 > 0 AND c1 >= c2)
);
```

* Set values in c2 column not NULL

```sql
CREATE TABLE t(
    c1 INT PRIMARY KEY,
    c2 VARCHAR NOT NULL
);
```

## Using SQL Operators

* Combine Rows From 2 Queries

```sql
SELECT c1,c2 FROM table_1
UNION [ALL]
SELECT c1,c2 FROM table_2;
```

* Return the intersection of 2 Queries

```sql
SELECT c1,c2 FROM table_1
INTERSECT
SELECT c1,c2 from table_2;
```

* Substract a result set from another Rasult Set

```sql
SELECT c1,c2 FROM table_1
MINUS
SELECT c1,c2 FROM table_2;
```

* Query Rows using Pattern Matching %

```sql
SELECT c1,c2 FROM table_1
WHERE c1 [NOT] LIKE pattern;
```

* Query Rows in A List

```sql
SELECT c1,c2 FROM table
WHERE c1 [NOT] IN value_list;
```

* Query Rows Between 2 values

```sql
SELECT c1,c2 FROM table
WHERE c1 BETWEEN low and high;
```

* Check if Values in a Table is NULL or Not

```sql
SELECT c1,c2 FROM table
WHERE c1 IS [NOT] NULL;
```

## SQL Intersect, Union, Union All and Except

![UNION Intersect Except](https://raw.githubusercontent.com/rca0/rca0.github.io/master/_posts/assets/union-intersect-except.png)

## Takeaways

Certainly, SQL includes many more statements than I mentioned in this post. As a DevOps/Infrastructure Engineer, I don't directly deal with complex queries, and these notes have been a great help in my day-to-day life—a quick guide for certain situations.

While these examples may be easy for many, this note serves as a handy reference for me to consult when I need to ensure that my queries are correct. There are many subjects related to databases that I hope to write about soon. :)

That's all, folks!
:) 
