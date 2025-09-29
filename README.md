# sql
Practicing SQL basics with sqlbolt.com

## Introduction to SQL
SQL (**Structured Query Language**) is a language designed to allow users to query, manipulate and transform data from a relational database.
Due to its simplicity, SQL databases provide safe and scalable storage for millions of websites and mobile applications. 

Some SQL databases: SQLite, MySQL, Postgres, Oracle and Microsoft SQL Server, all of them support the common SQL language standard. 

###  Relational Databases

A relational database represents a collection of related (two-dimensional) tables.

Each of the **tables** are similar to an Excel spreadsheet, with a fixed number of named columns (the attributes or properties of the table) and any number of rows of data.


## Lesson1: SELECT queries

To retrieve data from SQL database, we need to write `SELECT` statement, which are often colloquially refered to as queries.

A query in itself is just a statement which declares what data we are looking for, where to find it in the database, and optionally, how to transform it before it is returned.

You can think of a table in SQL as a type of an entity (ie. Dogs), and each row in that table as a specific instance of that type (ie. A pug, a beagle, a different colored pug, etc). This means that the columns would then represent the common properties shared by all instances of that entity (ie. Color of fur, length of tail, etc).

And given a table of data, the most basic query we could write would be one that selects for a couple columns (properties) of the table with all the rows (instances).

**Select query for a specific coloumns:**
```
SELECT coloumn, another_coloumn, ...
FROM mytable;
```

The result of this query will be a two-dimensional set of rows and columns, effectively a copy of the table, but only with the columns that we requested.

If we want to retrieve absolutely all the columns of data from a table, we can then use the asterisk (*) shorthand in place of listing all the column names individually.
This query, in particular, is really useful because it's a simple way to inspect a table by dumping all the data at once.

```
SELECT *
FROM mytable;
```

**Exercise1**

_Table: Movies_

1. Find the _title_ of each film:
```
SELECT title
FROM movies;
```
   
2. Find the _director_ of each film:
```
SELECT director 
FROM movies;
```

3. Find the _title_ and _director_ of each film:
```
SELECT title, director
FROM movies;
```

4. Find the _title_ and _year_ of each film:
```
SELECT title, year
FROM movies;
```

5. Find _all_ the information about each film:
```
SELECT *
FROM movies;
```

## Lesson2: Queries with constraints

In order to filter certain results from being returned, we need to use a **WHERE** clause in the query. The clause is applied to each row of data by checking specific column values to determine whether it should be included in the results or not.

Select query with constraints: 
```
SELECT column, another_column
FROM mytable
WHERE condition
  AND/OR another_condition
  AND/OR ...;
```

More complex clauses can be constructed by joining numerous AND or OR logical keywords (ie. num_wheels >= 4 AND doors <= 2). And below are some useful operators that you can use for numerical data (ie. integer or floating point):













