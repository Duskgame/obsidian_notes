---
aliases:
  - " "
  - delete keyword
---
[[Android]]
## Delete a row from a database

Finally, you can use a SQL `DELETE` statement to delete one or more rows from a table. A `DELETE` statement starts with the `DELETE` keyword, followed by the `FROM` keyword, followed by the table name, followed by a `WHERE` clause to specify which row or rows you want to delete.

```
DELETE FROM [Table Name]
WHERE [Condition];
```

Perform the following `DELETE` statement to delete the row with an `id` of `44` from the database.
```
DELETE FROM email
WHERE id = 44;
```

Validate your changes using a `SELECT` statement.
```
SELECT * FROM email
WHERE id = 44;
```

Observe that a row with an `id` of `44` no longer exists.