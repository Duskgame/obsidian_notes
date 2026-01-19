---
aliases:
  - " "
  - aggregate functions
---
## Reduce columns with aggregate functions

SQL statements aren't limited to returning rows. SQL offers a variety of functions that can perform an operation or calculation on a specific column, such as finding the maximum value, or counting the number of unique possible values for a particular column. These functions are called **aggregate functions**. Instead of returning all the data of a specific column, you can return a single value from a specific column.

Examples of SQL aggregate functions include the following:

- `COUNT()`: Returns the total number of rows that match the query.
- `SUM()`: Returns the sum of the values for all rows in the selected column.
- `AVG()`: Returns the mean value—average—of all the values in the selected column.
- `MIN()`: Returns the smallest value in the selected column.
- `MAX()`: Returns the largest value in the selected column.

Instead of a column name, you can call an aggregate function and pass in a column name as an argument between the parentheses.

```
SELECT [Aggregate Function] ([Column Name])
```

Instead of returning that column's value for every row in the table, a single value is returned from calling the aggregate function.

Aggregate functions can be an efficient way to perform calculations on a value when you don't need to read all the data in a database. For example, you might want to find the average of the values in a column without loading your entire database into a List and doing it manually.