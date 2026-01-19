---
aliases:
  - " "
  - insert statement
---
[[Android]]
## Insert data into a database

In addition to reading from a database, there are different SQL statements for writing to a database. The data needs a way to get in there in the first place, right?

You can add a new row to a database with an `INSERT` statement. An `INSERT` statement starts with `INSERT INTO` followed by the table name in which you want to insert a new row. The `VALUES` keyword appears on a new line followed by a set of parentheses that contain a comma separated list of values. You need to list the values in the same order of the database columns.

```
INSERT INTO [Table Name]
VALUES ((Values of Column 1), (Values of Column 2), ...);
```

Pretend the user receives a new email, and we need to store it in our app's database. We can use an `INSERT` statement to add a new row to the `email` table.

Perform an `INSERT` statement with the following data for a new email. Because the email is new, it is unread and initially appears in the inbox `folder`. A value of `NULL` is provided for the `id` column, which means the `id` will be automatically generated with the next available autoincremented integer..

```
INSERT INTO email
VALUES (
    NULL, 'Lorem ipsum dolor sit amet', 'sender@example.com', 'inbox', false, false, CURRENT_TIMESTAMP
);

```

**Note:** `CURRENT_TIMESTAMP` is a special variable that is replaced with the current time in [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) when the query runs, which is convenient for when you insert new rows!

Observe that the result is inserted into the database with an `id` of `44`.
