`@Embedded` is a Room annotation used in a query result class (not an entity) to tell Room to map the columns of another entity directly into that class as flat fields.

```kotlin
data class QuizWithQuestions(
    @Embedded val quiz: Quiz,  // all columns from the quiz table are mapped here
    ...
)
```

Room reads the `quiz` table row and maps it into a `Quiz` object normally — `@Embedded` just says "inline this object's columns here instead of in a separate query".

Used together with [[@Relation]] to build result classes that combine data from multiple tables.
