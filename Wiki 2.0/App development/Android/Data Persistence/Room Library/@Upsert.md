`@Upsert` is a Room annotation (available since Room 2.5) that combines insert and update into a single operation. If a row with the same primary key already exists it gets updated; if it doesn't exist yet it gets inserted.

```kotlin
@Upsert
suspend fun upsert(progress: QuestionProgress)
```

Useful when you don't know or don't want to check whether a record already exists — for example when recording user progress for a question that may or may not have been answered before.

Without `@Upsert` you would have to either use `@Insert(onConflict = OnConflictStrategy.REPLACE)` (which deletes and re-inserts, losing the row ID) or manually check and call insert or update separately.
