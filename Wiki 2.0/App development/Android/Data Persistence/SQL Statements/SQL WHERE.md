---
aliases:
  - " "
  - where clause
---
[[Android]]
## Filter queries with a `WHERE` clause

Many email apps offer the feature to filter the messages shown based on certain criteria, such as data, search term, folder, sender, etc. For these types of use cases, you can add a `WHERE` clause to your `SELECT` query.

After the table name, on a new line, you can add the `WHERE` keyword followed by an expression. When writing more complex SQL queries, it's common to put each clause on a new line for readability.

```
SELECT [Columns or Aggregate Functions] FROM [Table Name]
WHERE [Conditions];
```

```
SELECT * FROM email
WHERE folder = 'inbox';
```
(The result only returns rows for messages in the user's inbox.)


**Note:** Pay special attention to the SQL comparison operators!
Unlike in Kotlin, the comparison operator in SQL is a single equal sign (`=`), rather than a double equal sign (`==`).
The inequality operator (`!=`) is the same as in Kotlin. SQL also provides comparison operators `<`, `<=`, `>`, and `>=`.

## Logical operators with `WHERE` clauses

SQL `WHERE` clauses aren't limited to a single expression. You can use the `AND` keyword, 
equivalent to the Kotlin **and** operator (`&&`), to only include results that satisfy both conditions.

```
WHERE [First Condition] AND [First Condition]
```

Alternatively, you can use the `OR` keyword, equivalent to the Kotlin **or** operator (`||`), to include rows in the results that satisfy either condition.
```
WHERE [First Condition] OR [First Condition]
```

For readability, you can also negate an expression using the NOT keyword.
```
WHERE NOT [Condition]
```

**Note:** You can also write the SQL condition `NOT folder = 'spam'` as `folder != 'spam'`.

```
SELECT * FROM email
WHERE folder = 'inbox' AND read = false;
```
