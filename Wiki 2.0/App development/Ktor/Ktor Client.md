Ktor Client is the HTTP client library from the Ktor framework. It lets Android (or any Kotlin) apps make HTTP requests to a server. It's the counterpart to [[Ktor Server]] — the same ecosystem, used on the client side.

## Setup

Add dependencies in `libs.versions.toml`:
```toml
[versions]
ktor = "3.4.2"

[libraries]
ktor-client-core            = { module = "io.ktor:ktor-client-core", version.ref = "ktor" }
ktor-client-okhttp          = { module = "io.ktor:ktor-client-okhttp", version.ref = "ktor" }
ktor-client-content-negotiation = { module = "io.ktor:ktor-client-content-negotiation", version.ref = "ktor" }
ktor-client-serialization-json  = { module = "io.ktor:ktor-serialization-kotlinx-json", version.ref = "ktor" }
ktor-client-logging         = { module = "io.ktor:ktor-client-logging", version.ref = "ktor" }
```

- **core** — base API (required)
- **okhttp** — the underlying HTTP engine on Android (OkHttp is the standard Android HTTP library)
- **content-negotiation + serialization-json** — automatic JSON serialization/deserialization
- **logging** — prints request/response details to Logcat

Add the INTERNET permission and cleartext traffic flag to `AndroidManifest.xml` (see [[Internet Permissions]]).

## Creating the client

A single client instance is shared across the app (creating one per request is wasteful):

```kotlin
// data/remote/KwizzHttpClient.kt
val kwizzHttpClient = HttpClient(OkHttp) {
    install(ContentNegotiation) {
        json(Json { ignoreUnknownKeys = true })
    }
    install(Logging) {
        level = LogLevel.BODY
    }
}
```

`ignoreUnknownKeys = true` — if the server returns extra fields the client doesn't have a data class field for, don't crash. Useful when the API evolves.

## Making requests

All request functions are `suspend` — they must be called from a coroutine or another suspend function.

**GET:**
```kotlin
val quizzes: List<RemoteQuizWithQuestions> =
    kwizzHttpClient.get("http://10.0.2.2:8080/quizzes/browse").body()
```
`.body()` deserializes the JSON response body into the specified type automatically (requires ContentNegotiation + serialization).

**POST with a JSON body:**
```kotlin
kwizzHttpClient.post("http://10.0.2.2:8080/quizzes/upload") {
    contentType(ContentType.Application.Json)
    setBody(payload)  // payload is a @Serializable data class
}
```
`setBody()` serializes the Kotlin object to JSON. `contentType()` sets the `Content-Type: application/json` header so the server knows what format to expect.

## Separating remote models from local entities

Don't reuse Room `@Entity` classes as your API models. Keep separate `@Serializable` data classes for network traffic:

```kotlin
// data/remote/SyncModels.kt
@Serializable data class RemoteQuiz(val quizId: String, val name: String)
@Serializable data class RemoteQuestion(val questionId: String, val quizId: String, val question: String, val answer: String)
@Serializable data class RemoteQuizWithQuestions(val quiz: RemoteQuiz, val questions: List<RemoteQuestion>)
```

This way, changes to the database schema don't break the API contract, and vice versa.

## Related

- [[Ktor Server]]
- [[kotlinx.serialization]]
- [[HTTP Payload]]
- [[Emulator Networking]]
- [[REST]]
- [[Internet Permissions]]
