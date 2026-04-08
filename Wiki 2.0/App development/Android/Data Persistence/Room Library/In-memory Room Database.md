An **in-memory Room database** is a Room database that exists only in RAM. It is not written to disk and is destroyed as soon as the process ends (or the database is closed).

Used exclusively for testing — it gives you a real Room database with real SQL behaviour, without leaving files behind between test runs.

## How to create one

```kotlin
val database = Room.inMemoryDatabaseBuilder(context, KwizzDatabase::class.java)
    .allowMainThreadQueries()  // only acceptable in tests
    .build()
```

Compare with the production version:
```kotlin
Room.databaseBuilder(context, KwizzDatabase::class.java, "kwizz_database").build()
```

The only difference is `inMemoryDatabaseBuilder` vs `databaseBuilder` — no filename is needed.

## Typical test setup

```kotlin
@RunWith(AndroidJUnit4::class)
class KwizzRepositoryTest {

    private lateinit var database: KwizzDatabase
    private lateinit var repository: KwizzRepository

    @Before
    fun setup() {
        val context = ApplicationProvider.getApplicationContext<Context>()
        database = Room.inMemoryDatabaseBuilder(context, KwizzDatabase::class.java)
            .allowMainThreadQueries()
            .build()

        repository = OfflineKwizzRepository(
            quizDao = database.quizDao(),
            questionDao = database.questionDao(),
            answerHistoryDao = database.answerHistoryDao(),
            questionProgressDao = database.questionProgressDao()
        )
    }

    @After
    fun teardown() {
        database.close()  // destroys the in-memory DB
    }
}
```

## Where these tests live

In-memory database tests are [[Instrumented Tests]] — they require an Android device or emulator because Room needs an Android context.

They go in `app/src/androidTest/`, not `app/src/test/`.

## Related

- [[Room.md]]
- [[Database Repository]]
- [[Fake Repository]]
- [[Instrumented Tests]]
