`@Relation` is a Room annotation used in a query result class to automatically load a list of related child entities. Room runs a second query behind the scenes to fetch the children and populate the list.

```kotlin
data class QuizWithQuestions(
    @Embedded val quiz: Quiz,
    @Relation(
        parentColumn = "quizId",    // the column on the parent (Quiz) to match on
        entityColumn = "quiz_id"    // the column on the child (Question) that holds the foreign key
    )
    val questions: List<Question>
)
```

**How Room knows which table to query:** from the generic type — `List<Question>` tells Room to look in the `question` table, because `Question` is an `@Entity(tableName = "question")`.

**How it matches rows:** it compares the value of `parentColumn` on the parent row against `entityColumn` on every child row. Effectively runs:
```sql
SELECT * FROM question WHERE quiz_id = <quizId of this quiz>
```

The result class itself is never stored in the database — it is only a query result. Always used together with [[@Embedded]] and requires [[@Transaction]] on the DAO query method.
