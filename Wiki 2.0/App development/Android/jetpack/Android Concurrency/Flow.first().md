`Flow.first()` is a terminal operator from `kotlinx.coroutines.flow` that collects the first emitted value from a Flow and then cancels the collection. It is a suspend function, so it must be called from a coroutine or another suspend function.

```kotlin
val progressList = repository.getProgressForQuestions(questionIds).first()
```

Useful when you need a **one-shot snapshot** of data from a Flow rather than continuously observing it. For example, loading the current state of database records at a specific point in time (like when a quiz starts) rather than reacting to every future change.

Contrast with `collect {}`, which keeps running and reacts to every new emission — appropriate for UI state that should update continuously, not for one-time reads inside a function.
