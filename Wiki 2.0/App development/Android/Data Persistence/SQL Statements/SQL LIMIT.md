---
aliases:
  - " "
  - limit
---
[[Android]]
## Restrict the number of results with `LIMIT`

So far, all the examples return every single result from the database that matches the query. In many cases, you only need to display a limited number of rows from your database. You can add a `LIMIT` clause to your query to only return a specific number of results. Add the `LIMIT` keyword followed by the maximum number of rows you want to return. If applicable, the `LIMIT` clause comes after the `ORDER BY` clause.

```
LIMIT [Max Rows to return]
```

Optionally, you can include the `OFFSET` keyword followed by another number for the number of rows to "skip". For example, if you want the next ten results, after the first ten, but don't want to return all twenty results, you can use `LIMIT 10 OFFSET 10`.

```
LIMIT [Max Rows to return] OFFSET [Rows to skip]
```

In an app, you might want to load emails more quickly by only returning the first ten emails in the user's inbox. Users can then scroll to view subsequent pages of emails. The following instructions use a `LIMIT` clause to achieve this behavior.

Perform the following `SELECT` statement to get all emails in the user's inbox in descending order and limited to the first ten results.
```
SELECT * FROM email
WHERE folder = 'inbox'
ORDER BY received DESC
LIMIT 10;
```
Observe that only ten results are returned.

Modify and re-run the query to include the `OFFSET` keyword with a value of `10`.
```
SELECT * FROM email
WHERE folder = 'inbox'
ORDER BY received DESC
LIMIT 10 OFFSET 10;
```
The query returns ten results in decreasing order. However, the query skips the first set of ten results.
