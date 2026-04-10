**Exposed** is a Kotlin SQL library by JetBrains. It lets you define database tables and write queries in Kotlin instead of raw SQL strings.

## Dependencies

```kotlin
val exposedVersion = "0.55.0"
implementation("org.jetbrains.exposed:exposed-core:$exposedVersion")
implementation("org.jetbrains.exposed:exposed-jdbc:$exposedVersion")
implementation("com.h2database:h2:2.2.224")       // local development
// implementation("org.postgresql:postgresql:42.7.3") // production
```

- `exposed-core` — table DSL and query builders
- `exposed-jdbc` — connects Exposed to a [[JDBC]] database driver
- The database driver (h2 or postgresql) is swapped depending on environment

## Connecting to the database

```kotlin
object DatabaseFactory {
    fun init() {
        Database.connect(
            url = "jdbc:h2:file:./data/kwizz;DB_CLOSE_DELAY=-1",
            driver = "org.h2.Driver"
        )
        transaction {
            SchemaUtils.create(QuizTable, QuestionTable)
        }
    }
}
```

`SchemaUtils.create(...)` creates tables if they don't exist yet — safe to call on every startup.

## Defining tables

Tables are `object` singletons that extend `Table`:

```kotlin
object QuizTable : Table("quiz") {
    val quizId = varchar("quiz_id", 36)
    val name = varchar("name", 255)
    override val primaryKey = PrimaryKey(quizId)
}

object QuestionTable : Table("question") {
    val questionId = varchar("question_id", 36)
    val quizId = varchar("quiz_id", 36) references QuizTable.quizId  // foreign key
    val question = varchar("question", 1000)
    val answer = varchar("answer", 255)
    override val primaryKey = PrimaryKey(questionId)
}
```

`references` creates a foreign key — the database enforces that every question's `quizId` must match an existing quiz.

## Transactions

Every database operation must happen inside a `transaction {}` block. A transaction is a unit of work: either everything succeeds and is committed, or it all rolls back on failure.

```kotlin
transaction {
    QuizTable.insert {
        it[quizId] = "q1"
        it[name] = "Geography"
    }
}
```

## Coroutine-safe transactions in Ktor

Database calls are blocking (they wait for I/O). In Ktor routes, use `newSuspendedTransaction(Dispatchers.IO)` to move the blocking work off the main coroutine dispatcher:

```kotlin
get("/quizzes") {
    val quizzes = newSuspendedTransaction(Dispatchers.IO) {
        QuizTable.selectAll().map {
            Quiz(quizId = it[QuizTable.quizId], name = it[QuizTable.name])
        }
    }
    call.respond(quizzes)
}
```

`Dispatchers.IO` is a thread pool designed for blocking operations. Using it prevents the server from freezing while waiting on the database.

## Common queries

```kotlin
// Select all
QuizTable.selectAll()

// Select with filter
QuestionTable.selectAll().where { QuestionTable.quizId eq "q1" }

// Insert
QuizTable.insert {
    it[quizId] = quiz.quizId
    it[name] = quiz.name
}

// Update — returns the number of rows affected
QuizTable.update({ QuizTable.quizId eq "q1" }) {
    it[name] = "New Name"
}

// Delete — also returns the number of rows affected
QuizTable.deleteWhere { quizId eq "q1" }
```

## Checking affected rows

`update()` and `deleteWhere()` both return an `Int` — the number of rows that were changed. Use this to return a proper `404` when the client targets something that doesn't exist:

```kotlin
val updated = QuizTable.update({ QuizTable.quizId eq id }) {
    it[name] = quiz.name
}
if (updated == 0) {
    call.respond(HttpStatusCode.NotFound, "Quiz not found")
} else {
    call.respond(HttpStatusCode.OK, quiz)
}
```

Without this check, a `PUT` to a non-existent ID would silently do nothing and still return `200 OK` — misleading the client into thinking the update succeeded.

## Cascade delete and foreign key constraints

When a table has a foreign key referencing another table, the database will **reject** deleting the parent row if child rows still reference it. You must delete the children first:

```kotlin
// ❌ This fails if questions exist for this quiz
QuizTable.deleteWhere { quizId eq id }

// ✅ Delete children first, then the parent — all in one transaction
newSuspendedTransaction(Dispatchers.IO) {
    QuestionTable.deleteWhere { quizId eq id }  // children first
    QuizTable.deleteWhere { quizId eq id }      // then parent
}
```

Wrapping both deletes in a single transaction is important — if the second delete fails for any reason, the first one rolls back too. The database is never left in a half-deleted state.

## H2 vs PostgreSQL

H2 is a database that runs inside the JVM — no installation needed, good for local development. PostgreSQL is a full server database used in production. Switching is a one-line connection URL change:

```kotlin
// H2 (local)
url = "jdbc:h2:file:./data/kwizz;DB_CLOSE_DELAY=-1"
driver = "org.h2.Driver"

// PostgreSQL (production)
url = "jdbc:postgresql://localhost:5432/kwizz"
driver = "org.postgresql.Driver"
```

## Related

- [[Ktor Server]]
- [[JDBC]]
- [[REST]]
- [[Database]]
