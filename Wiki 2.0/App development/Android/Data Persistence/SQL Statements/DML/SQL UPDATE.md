---
aliases:
  - " "
  - update keyword
---
[[Android]]
## Update existing data in a database

After you've inserted data into a table, you can still change it later. You can update the value of one or more columns using an `UPDATE` statement. An `UPDATE` statement starts with the `UPDATE` keyword, followed by the table name, followed by a `SET` clause.

```
UPDATE [Table Name]
SET [Sets of Columns and Values];
```

A SET clause consists of the SET keyword, followed by the name of the column you want to update.

```
SET [First Coolumn] = [First Value],
[Second Column] = [Second Value],
...
```

An `UPDATE` statement often includes a `WHERE` clause to specify the single row or multiple rows that you want to update with the specified column-value pair.

```
UPDATE [Table Name]
SET [Sets of Columns and Values]
WHERE [Condition(s)];
```

If the user wants to mark an email as read, for example, you use an `UPDATE` statement to update the database. The following instructions let you mark the email inserted in the previous step as read.

Perform the following `UPDATE` statement to set the row with an `id` of `44` so that the value of the `read` column is `true`.
```
UPDATE email
SET read = true
WHERE id = 44;
```

Run a `SELECT` statement for that specific row to validate the result.
```
SELECT read FROM email
WHERE id = 44;
```

Observe that the value of the read column is now `1` for a "true" value as opposed to `0` for "false"..