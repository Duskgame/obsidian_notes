**kotlinx.serialization** is JetBrains' official Kotlin library for converting Kotlin objects to and from formats like JSON. It's the standard serialization library for Kotlin and is used by both Ktor Server and Ktor Client.

## @Serializable

Mark a data class with `@Serializable` to make it serializable:

```kotlin
@Serializable
data class Quiz(
    val quizId: String,
    val name: String
)
```

The compiler plugin (applied in `build.gradle.kts`) generates the serialization code at compile time. You don't write it yourself.

## Setup

In `build.gradle.kts`:
```kotlin
plugins {
    id("org.jetbrains.kotlin.plugin.serialization") version "2.x.x"
}
dependencies {
    implementation("io.ktor:ktor-serialization-kotlinx-json:$ktor_version")
}
```

The **plugin** makes `@Serializable` work (generates code). The **library** provides the `Json` encoder and integration with Ktor.

## JSON encoding/decoding

```kotlin
val json = Json { ignoreUnknownKeys = true }

// Kotlin object → JSON string
val encoded = json.encodeToString(quiz)

// JSON string → Kotlin object
val decoded = json.decodeFromString<Quiz>(jsonString)
```

In practice with Ktor Client, you don't call these manually — `ContentNegotiation` calls them automatically when you use `setBody()` and `.body()`.

## ignoreUnknownKeys

```kotlin
Json { ignoreUnknownKeys = true }
```

If the JSON from the server has fields your data class doesn't have, ignore them instead of throwing an exception. Important for forward compatibility — if the API adds a new field, old app versions won't crash.

## Nested objects

Works automatically — nest `@Serializable` classes inside each other:

```kotlin
@Serializable
data class RemoteQuizWithQuestions(
    val quiz: RemoteQuiz,
    val questions: List<RemoteQuestion>
)
```

The whole object tree is serialized/deserialized as a single JSON object.

## Related

- [[Ktor Client]]
- [[Ktor Server]]
- [[JSON]]
