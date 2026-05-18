---
aliases:
  - " "
  - order by
---
[[Android]]
## Sort results with `ORDER BY`

You can also change the order of query results when you sort them with the `ORDER BY` clause. Add the `ORDER BY` keyword, followed by a column name, followed by the sort direction.

```
ORDER BY [Column Name] [Sort Direction]
```

By default, the sort direction is ascending order, which you can omit from the `ORDER BY` clause. If you want the results sorted in descending order, add `DESC` after the column name.

Chances are you expect an email app to show the most recent emails first. The following instructions let you do this with an `ORDER BY` clause.

Add an `ORDER BY` clause to sort unread emails, based on the `received` column. Because ascending order—lowest or the oldest first—is the default, you need to use the `DESC` keyword.

```
SELECT * FROM email
ORDER BY received DESC;
```


You can use an `ORDER BY` clause with a `WHERE` clause. Say a user wants to search for old emails containing the text _fool_. They can sort the results to show the oldest emails first, in ascending order.

Select all emails where the subject contains the text "fool" and sort the results in ascending order. Because the order is ascending, which is the default order when none is specified, using the `ASC` keyword with the `ORDER BY` clause is optional.

```
SELECT * FROM email
WHERE subject LIKE '%fool%'
ORDER BY received ASC;
```

Observe that the filtered results are returned with the oldest—lowest value in the received column—shown first.

**Note:** If both are used in the same query, the `GROUP BY` clause comes before the `ORDER BY` clause.

