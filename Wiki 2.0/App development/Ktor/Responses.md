https://ktor.io/docs/server-responses.html

[[Ktor]] allows you to handle incoming [requests](https://ktor.io/docs/server-requests.html) and send responses inside [route handlers](https://ktor.io/docs/server-routing.html#define_route). You can send different types of responses: plain text, HTML documents and templates, serialized data objects, and so on. For each response, you can also configure various [response parameters](https://ktor.io/docs/server-responses.html#parameters), such as a content type, headers, and cookies.

Inside a route handler, the following [[API]] is available for working with responses:

- A set of functions targeted for [sending a specific content type](https://ktor.io/docs/server-responses.html#payload), for example, [call.respondText](https://api.ktor.io/ktor-server-core/io.ktor.server.response/respond-text.html), [call.respondHtml](https://api.ktor.io/ktor-server-html-builder/io.ktor.server.html/respond-html.html), and so on.
    
- The [call.respond](https://api.ktor.io/ktor-server-core/io.ktor.server.response/respond.html) function that allows you to [send any data](https://ktor.io/docs/server-responses.html#payload) inside a response. For example, with the enabled [ContentNegotiation](https://ktor.io/docs/server-serialization.html) plugin, you can send a data object serialized in a specific format.
    
- The [call.response](https://api.ktor.io/ktor-server-core/io.ktor.server.application/-application-call/response.html) property that returns the [ApplicationResponse](https://api.ktor.io/ktor-server-core/io.ktor.server.response/-application-response/index.html) object that provides access to [response parameters](https://ktor.io/docs/server-responses.html#parameters) and allows you to set the status code, add headers, and configure cookies.
    
- The [call.respondRedirect](https://api.ktor.io/ktor-server-core/io.ktor.server.response/respond-redirect.html) provides the capability to add redirects.


To send a template in a response, call the [call.respond](https://api.ktor.io/ktor-server-core/io.ktor.server.response/respond.html) function with a specific content ...

```
get("/index") {
    val sampleUser = User(1, "John")
    call.respond(FreeMarkerContent("index.ftl", mapOf("user" to sampleUser)))
}
```