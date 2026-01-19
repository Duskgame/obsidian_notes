---
aliases:
  - " "
  - group by
---
You just saw how to use aggregate functions and the WHERE clause to filter and reduce results. SQL offers several other clauses that can help you format the results of your query. Among these clauses are grouping, ordering, and limiting results.

You can use a GROUP BY clause to group results so that all rows that have the same value for a given column are grouped next to each other in the results. This clause doesn't change the results, but only the order in which they're returned.

To add a GROUP BY clause to a SELECT statement, add the GROUP BY keyword followed by the column name you want to group results by.

```
GROUP BY [Column Name]
```

A common use case is to couple a `GROUP BY` clause with an aggregate function to partition the result of the aggregate function across different buckets, such as values of a column. Here's an example. Pretend you want to get the number of emails in each folder: `'inbox'`, `'spam'`, etc. You can select both the `folder` column and the `COUNT()` aggregate function, and specify the `folder` column in the `GROUP BY` clause.


Perform the following query to select the folder column, and the result of `COUNT()` aggregate function. Use a `GROUP BY` clause to bucket the results by the value in the `folder` column.
```
SELECT folder, COUNT(*) FROM email
GROUP BY folder;
```
The query returns the total number of emails for each folder.

**Note:** You can specify multiple columns, separated by a comma in the `GROUP BY` clause, if you want to further separate each group into additional subgroups based on a different column.