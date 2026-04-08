A **Fake Repository** is a test implementation of a [[Database Repository|repository interface]] that returns hardcoded data instead of hitting a real database. It lets you run and test app logic without needing a database at all.

## Why it works

Because your repository is defined as an **interface** (`KwizzRepository`), any class that implements it can be swapped in. The rest of the app (ViewModels, UI) doesn't know or care whether it's talking to a real or fake implementation.

This is the key benefit of the repository pattern — the interface acts as a **seam** where you can swap implementations.

## Structure

```kotlin
class FakeKwizzRepository : KwizzRepository {

    private val quizzes = MutableStateFlow(
        listOf(
            QuizWithQuestions(
                quiz = Quiz(quizId = "q1", name = "Geography"),
                questions = listOf(
                    Question(questionId = "q1_1", quizId = "q1", question = "Capital of France?", answer = "Paris")
                )
            )
        )
    )

    override fun getAllQuizzesWithQuestions(): Flow<List<QuizWithQuestions>> = quizzes

    // No-ops for write operations
    override suspend fun insertQuiz(quiz: Quiz) {}
    override suspend fun deleteQuiz(quiz: Quiz) {}
    // ...
}
```

## How to use it in the app

Pair it with a `FakeAppContainer` and swap it in `KwizzApplication`:

```kotlin
class FakeAppContainer : AppContainer {
    override val repository: KwizzRepository = FakeKwizzRepository()
}

// In KwizzApplication.onCreate():
container = FakeAppContainer()  // swap back to AppDataContainer(this) for production
```

## Use cases

- **Running the app on an emulator with no real data** — see the full UI flow without seeding a database
- **ViewModel unit tests** — inject the fake directly and assert on state changes
- **Compose Previews** — pass fake data to preview screens in isolation

## Related

- [[Database Repository]]
- [[Database AppContainer]]
- [[In-memory Room Database]]
