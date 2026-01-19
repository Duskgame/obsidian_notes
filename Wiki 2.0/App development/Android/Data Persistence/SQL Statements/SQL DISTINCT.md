---
aliases:
  - " "
  - distinct
---
[[Android]]
## Filter duplicate results with `DISTINCT`

When you select a column, you can precede it with the `DISTINCT` keyword. This approach can be useful if you want to remove duplicates from the query result.
```
SELECT DISTINCT [Column Name] FROM [Table Name];
```

```
SELECT DISTINCT sender FROM email;
```

Notice that the result is now much smaller and every value is unique.


You can also precede the column name in an aggregate function with the `DISTINCT` keyword.
```
SELECT [Aggregate Function] (DISTINCT[Column Name])
FROM [Table Name];
```

```
SELECT COUNT(DISTINCT sender) FROM email;
```
(Observe that the query tells us that there are 14 unique senders.)