---
layout: post
title: SQL Concepts
short-description: One brief introdution about SQL. SQL stands for Structured Query Language, whereas you can access, manipulate...
date: 2023-11-20
---

# WIP - SQL Concepts

For me SQL it is always hard to do when things goes beyond the simple queries (i'm not used to execute SQL in my daily work). Everytime when i need to make complex queries i have to spend a lot of time doing research. Now, i would like to share one thing that has been helping me a lot when i need to use SQL.

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

* Differences Between INNER,LEFT,RIGHT and FULL JOIN

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

---

# Managing SQL

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

## Takeaways

For sure, SQL has so much more statements than i mentioned in this post. As DevOps/Infrastructure Engineer i do not work directly with complex queries and these notes have helped me a lot in my day-to-day life as quick guide for certain situation.
