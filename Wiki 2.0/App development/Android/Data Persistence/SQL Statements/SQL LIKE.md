---
aliases:
  - " "
  - like keyword
---
[[Android]]
## Search for text using `LIKE`

One super useful thing you can do with a `WHERE` clause is to search for text in a specific column. You achieve this result when you specify a column name, followed by the `LIKE` keyword, followed by a search string.

```
WHERE [Column Name] LIKE [Search String]
```

The search string starts with the percent symbol (`%`), followed by the text to search for (Search term), followed by the percent symbol (`%`) again.

```
LIKE `%[Search Term]%`
```

If you're searching for a prefix—results that begin with the specified text—omit the first percent symbol (`%`).
```
LIKE `[Search Term]%`
```

Alternatively, if you're searching for a suffix, omit the last percent symbol (`%`).
```
LIKE `%[Search Term]`
```

There are many use cases where an app can use text search, such as searching for emails that contain particular text in the subject line or updating autocomplete suggestions as the user is typing.



Run the following query to get the total number of emails with the text "fool" in the subject line.
```
SELECT COUNT(*) FROM email
WHERE subject LIKE '%fool%';
```

Run the following query to return all columns from all rows where the subject ends with the word fool.
```
SELECT * FROM email
WHERE subject LIKE '%fool';
```

Run the following query to return distinct values of the `sender` column that begin with the letter `h`.
```
SELECT DISTINCT sender FROM email
WHERE sender LIKE 'h%';
```
