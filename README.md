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

![alt text](images/sql.png)

In addition to making the results **more manageable** to understand, writing clauses to constrain the set of rows returned also allows the query to **run faster due to the reduction in unnecessary data** being returned.


**Exercise2**

_Table: Movies_

1. Find the movie with a row id of 6
```
SELECT title
FROM movies
WHERE id=6;
```

2. Find the movies released in the years between 2000 and 2010
```
SELECT title
FROM movies
WHERE year BETWEEN 2000 AND 2010;
```

3. Find the movies not released in the years between 2000 and 2010
```
SELECT title
FROM movies
WHERE year NOT BETWEEN 2000 AND 2010;
```

4. Find the first 5 Pixar movies and their release year
```
SELECT title, year
FROM movies
WHERE id<6;
```

## Lesson3: Queries with constraints 2

When writing **WHERE** clauses with columns containing text data, SQL supports a number of useful operators to do things like case-insensitive string comparison and wildcard pattern matching. We show a few common text-data specific operators below:


![alt text](images/sql2.png)

All strings must be quoted so that the query parser can distinguish words in the string from SQL keywords.

**Exercise3**

_Table: Movies_

1. Find all the Toy Story movies
```
SELECT title
FROM movies
WHERE title LIKE "%Toy Story%";
```

2. Find all the movies directed by John Lasseter
```
SELECT title
FROM movies
WHERE director="John Lasseter";
```

3. Find all the movies (and director) not directed by John Lasseter
```
SELECT title, director
FROM movies
WHERE director!="John Lasseter";
```

4. Find all the WALL-* movies
```
SELECT *
FROM movies
WHERE title LIKE "WALL-_";
```

## Lesson4: Filtering and sorting query results 

DISTINCT keyword: provides a convenient way to discard rows that have a duplicate column value.

Select query with unique results
```
SELECT DISTINCT column, another_column
FROM mytable
WHERE condition;
```

**Ordering results**

SQL provides a way to sort your results by a given column in ascending or descending order using the **ORDER BY** clause.

Select query with ordered results:
```
SELECT column, another_column
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC;
```

When an ORDER BY clause is specified, each row is sorted alpha-numerically based on the specified column's value.

**Limiting results to a subset**

Another clause which is commonly used with the ORDER BY clause are the **LIMIT** and **OFFSET** clauses, which are a useful optimization to indicate to the database the subset of the results you care about.

Select query with limited rows:
```
SELECT column, another_column
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

**Exercise4**

_Table: Movies_

1. List all directors of Pixar movies (alphabetically), without duplicates
```
SELECT DISTINCT director
FROM movies
ORDER BY director ASC;
```

2. List the last four Pixar movies released (ordered from most recent to least)
```
SELECT *
FROM movies
ORDER BY year DESC
LIMIT 4;
```

3. List the first five Pixar movies sorted alphabetically 
```
SELECT * 
FROM movies
ORDER BY title ASC
LIMIT 5;
```

4. List the next five Pixar movies sorted alphabetically
```
SELECT *
FROM movies
ORDER BY title ASC
LIMIT 5 OFFSET 5;
```

## Review simple SELECT queries
```
SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

## Lesson6: Multi-table queries with JOINs

Tables that share information about a single entity need to have a primary key that identifies that entity uniquely across the database. One common primary key type is an auto-incrementing integer (because they are space efficient), but it can also be a string, hashed value, so long as it is unique.

Using the *JOIN* clause in a query, we can combine row data across two separate tables using this unique key. The first of the joins that we will introduce is the *INNER JOIN*.

```
Select query with INNER JOIN on multiple tables:

SELECT column, another_column
FROM mytable
INNER JOIN another_table 
    ON mytable.id = another_table.id
WHERE condition(s)
ORDER BY column ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

The *INNER JOIN* is a process that matches rows from the first table and the second table which have the same key (as defined by the ON constraint) to create a result row with the combined columns from both tables. After the tables are joined, the other clauses we learned previously are then applied.

**Exercise6**

1. Find the domestic and international sales for each movie 
```
SELECT title, domestic_sales, international_sales
FROM movies
INNER JOIN boxoffice
  ON movies.id=boxoffice.movie_id;
```

2. Show the sales numbers for each movie that did better internationally rather than domestically 
```
SELECT title, domestic_sales, international_sales 
FROM movies
INNER JOIN boxoffice
    ON movies.id=boxoffice.movie_id
WHERE international_sales > domestic_sales;
```
3. List all the movies by their ratings in descending order 
```
SELECT title, rating 
FROM movies
INNER JOIN boxoffice
    ON movies.id = boxoffice.movie_id
ORDER BY rating DESC;
```

## Lesson7: Other JOINs

If the two tables have asymmetric data, which can easily happen when data is entered in different stages, then we would have to use a *LEFT JOIN*, *RIGHT JOIN* or *FULL JOIN* instead to ensure that the data you need is not left out of the results.

Select query with LEFT/RIGHT/FULL JOINs on multiple tables:

```
SELECT column, another_column, …
FROM mytable
INNER/LEFT/RIGHT/FULL JOIN another_table 
    ON mytable.id = another_table.matching_id
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

Like the INNER JOIN these three new joins have to specify which column to join the data on.
When joining table A to table B, a LEFT JOIN simply includes rows from A regardless of whether a matching row is found in B. The RIGHT JOIN is the same, but reversed, keeping rows in B regardless of whether a match is found in A. Finally, a FULL JOIN simply means that rows from both tables are kept, regardless of whether a matching row exists in the other table.

**Exercise7**

1. Find the list of all buildings that have employees
```
SELECT DISTINCT building
FROM employees;
```

2. Find the list of all buildings and their capacity

3. List all buildings and the distinct employee roles in each building (including empty buildings) 

```
SELECT DISTINCT building_name, role 
FROM buildings 
LEFT JOIN employees
    ON building_name = building;
```

## Lesson8: NULL 

we are going to quickly talk about NULL values in an SQL database.
It's always good to reduce the possibility of NULL values in databases because they require special attention when constructing queries, constraints (certain functions behave differently with null values) and when processing the results.

An alternative to NULL values in your database is to have *data-type appropriate default values*, like 0 for numerical data, empty strings for text data, etc. But if your database needs to store incomplete data, then NULL values can be appropriate if the default values will skew later analysis (for example, when taking averages of numerical data).

Sometimes, it's also not possible to avoid NULL values, as we saw in the last lesson when outer-joining two tables with asymmetric data. In these cases, you can test a column for NULL values in a WHERE clause by using either the IS NULL or IS NOT NULL constraint.

```
Select query with constraints on NULL values: 

SELECT column, another_column, …
FROM mytable
WHERE column IS/IS NOT NULL
AND/OR another_condition
AND/OR …;
```

**Exercise8**

1. Find the name and role of all employees who have not been assigned to a building 
```
SELECT name, role 
FROM employees
WHERE building IS NULL;
```

2. Find the names of the buildings that hold no employees

```
SELECT DISTINCT building_name
FROM buildings 
  LEFT JOIN employees
    ON building_name = building
WHERE role IS NULL;
```


## Lesson 17: Altering tables 

SQL provides a way for you to update your corresponding tables and database schemas by using the ALTER TABLE statement to add, remove, or modify columns and table constraints.

#### Adding coloumns

You need to specify the data type of the column along with any potential table constraints and default values to be applied to both existing and new rows. In some databases like MySQL, you can even specify where to insert the new column using the FIRST or AFTER clauses, though this is not a standard feature.
```
ALTER TABLE mytable
ADD column DataType OptionalTableConstraint 
    DEFAULT default_value;
```

#### Removing coloumns
```
ALTER TABLE mytable
DROP column_to_be_deleted;
```

#### Renaming the table
```
ALTER TABLE mytable
RENAME TO new_table_name;
```

**Exercise17:**
1. Add a column named Aspect_ratio with a FLOAT data type to store the aspect-ratio each movie was released in.
```
ALTER TABLE movies
ADD column Aspect_ratio FLOAT DEFAULT 0.0;
```

2. Add another column named Language with a TEXT data type to store the language that the movie was released in. Ensure that the default for this language is English.
```
ALTER TABLE movies
ADD column Language TEXT DEFAULT "English";
```


## Lesson 18: Dropping tables 

To remove an entire table including all of its data and metadata, and to do so, you can use the *DROP TABLE* statement, which differs from the *DELETE* statement in that it also removes the table schema from the database entirely.

```
DROP TABLE IF EXISTS mytable;
```

Like the CREATE TABLE statement, the database may throw an error if the specified table does not exist, and to suppress that error, you can use the *IF EXISTS* clause.

In addition, if you have another table that is dependent on columns in table you are removing (for example, with a FOREIGN KEY dependency) then you will have to either update all dependent tables first to remove the dependent rows or to remove those tables entirely.


 Questions:

 Database normalization









