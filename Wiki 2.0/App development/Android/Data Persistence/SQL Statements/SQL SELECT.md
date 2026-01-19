---
aliases:
  - " "
  - select
---
## SQL `SELECT` statement

A SQL statement—sometimes called a query—is used to read or manipulate a database.

You read data from a SQLite database with a `SELECT` statement. A simple `SELECT` statement consists of the `SELECT` keyword, followed by the column name, followed by the `FROM` keyword, followed by the table name. Every SQL statement ends with a semicolon (`;`).
```
SELECT (Column Name) FROM (Table Name);
```

A `SELECT` statement can also return data from multiple columns. You must separate column names with a comma.
```
SELECT (Column 1), (Column 2) FROM (Table Name);
```

If you want to select every column from the table, you use the wildcard character (`*`) in place of the column names.
```
SELECT * FROM (Table Name);
```


In either case, a simple `SELECT` statement like this returns every row in the table. You just need to specify the column names you want it to return.

**Note:** While it is the convention to end every SQL statement with a semicolon (`;`), certain editors like the database inspector in Android Studio might let you omit the semicolon. The diagrams in this codelab show a semicolon at the end of each complete SQL query.