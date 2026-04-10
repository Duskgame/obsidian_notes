A Ktor server is a standalone JVM application — a separate project from the Android app. It receives HTTP requests and responds with data (typically JSON).

## Project generation

Use **start.ktor.io** to generate the project. Key choices:
- **Engine:** Netty — the standard async HTTP engine for Ktor
- **Plugins:** Routing, Content Negotiation (+ kotlinx.serialization), Status Pages

## Project structure

```
kwizz-backend/
├── build.gradle.kts
└── src/main/kotlin/com/example/
    ├── Application.kt         ← entry point
    ├── plugins/
    │   ├── Routing.kt         ← registers all routes
    │   └── Serialization.kt   ← configures JSON
    └── routes/
        └── QuizRoutes.kt      ← route definitions per feature
```

## Application.kt — entry point

```kotlin
fun main(args: Array<String>) {
    io.ktor.server.netty.EngineMain.main(args)
}

fun Application.module() {
    DatabaseFactory.init()   // set up DB before routes
    configureSerialization()
    configureRouting()
}
```

`module()` is called on startup — equivalent to `onCreate()` in Android. Order matters: database must be ready before routes start accepting requests.

## application.yaml — server config

```yaml
ktor:
    application:
        modules:
            - com.example.ApplicationKt.module
    deployment:
        port: 8080
```

Tells Ktor which `module()` function to call and which port to listen on. Port 8080 is the conventional non-privileged development port.

## Plugins

Plugins are opt-in features installed into the application:

**Content Negotiation** — automatically serializes Kotlin objects to JSON in responses, and deserializes JSON bodies into Kotlin objects on incoming requests:
```kotlin
install(ContentNegotiation) { json() }
```

**Status Pages** — catches unhandled exceptions and returns a proper error response instead of crashing silently:
```kotlin
install(StatusPages) {
    exception<Throwable> { call, cause ->
        call.respondText("500: $cause", status = HttpStatusCode.InternalServerError)
    }
}
```

## Route extension functions

The Ktor convention for organising routes is extension functions on `Route`. Each feature area gets its own file:

```kotlin
// routes/QuizRoutes.kt
fun Route.quizRoutes() {
    route("/quizzes") {
        get { ... }
        post { ... }
        delete("/{id}") { ... }
    }
}

// plugins/Routing.kt
fun Application.configureRouting() {
    routing {
        quizRoutes()       // register all quiz routes with one line
        questionRoutes()   // easy to add more feature areas
    }
}
```

## Running locally

Run `Application.kt → main()` in IntelliJ. Server starts on `http://localhost:8080`.

Test GET routes in a browser. For POST/PUT/DELETE, use a REST client like **Bruno** or **Postman** — they let you set headers, send request bodies, and inspect status codes.

## Related

- [[Ktor]]
- [[REST]]
- [[Exposed]]
- [[Path parameters]]
- [[Responses]]
