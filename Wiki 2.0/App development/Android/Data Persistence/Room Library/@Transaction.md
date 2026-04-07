`@Transaction` is a Room annotation placed on a DAO query method to wrap its execution in a single database transaction.

Required whenever a query internally triggers multiple SQL statements — most commonly when using [[@Relation]], where Room runs one query for the parent and a second for each child list.

```kotlin
@Transaction
@Query("SELECT * FROM quiz")
fun getAllQuizzesWithQuestions(): Flow<List<QuizWithQuestions>>
```

Without `@Transaction`, the database could change between the two reads and return inconsistent data (e.g. a quiz with questions that don't belong to it). With it, both reads see the same snapshot of the database.
